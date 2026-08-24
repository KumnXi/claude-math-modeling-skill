---
name: math-modeling-solver
description: Solve math modeling competition problems (Huazhong Cup, CUMCM, etc.) with a systematic workflow — data exploration, model building, algorithm implementation, result analysis.
---

# Math Modeling Problem Solver

Use this skill when the user asks to solve a mathematical modeling competition problem, especially multi-part problems like the Huazhong Cup or CUMCM.

## Workflow

### Phase 1: Understand the Problem

1. **Read all problem descriptions and data files** — Identify each sub-problem, its constraints, objectives, and how it differs from other parts
2. **Load and inspect data files** — Check Excel/CSV contents, understand column meanings, identify data types (coordinates, time windows, orders, distance matrices)
3. **Extract all model parameters** — List every constant, coefficient, and constraint from the problem statement (speeds, penalty rates, emission factors, etc.)

### Phase 2: Design the Solution Architecture

1. **Define the directory structure**:
   ```
   problem_A/code/   ← solver scripts
   problem_A/data/   ← input data (xlsx, csv)
   problem_A/figures/ ← visualizations
   problem_B/...     ← same pattern for each sub-problem
   ```
2. **Choose the modeling approach** — e.g., Mixed Integer Programming, heuristic algorithms, dynamic programming
3. **Design the solver pipeline**:
   - Data loading and preprocessing (load Excel → normalize → index)
   - Initial solution construction (greedy, insertion heuristics)
   - Solution improvement (local search: 2-opt, swap, relocate)
   - Constraint checking (capacity, time windows, special restrictions)
   - Cost calculation (multi-component: startup + fuel + carbon + penalty)
4. **Plan output format** — JSON for structured results + TXT for human-readable reports

### Phase 3: Implement the Solver

1. **Use Python with pandas, numpy** — Load Excel data, build distance matrices
2. **Build cost functions carefully**:
   - Time-varying speeds → time-dependent travel times
   - U-shaped energy curves → speed-dependent fuel/kWh consumption
   - Load-dependent adjustments → linear interpolation between empty and full
   - Soft time windows → early/late penalty with different rates
   - Carbon pricing → fuel-to-CO₂ conversion × carbon price
3. **Implement greedy construction** — Sort customers by priority (time window start), insert at minimum marginal cost position
4. **Implement 2-opt local search** — Iterate until no improvement; check all possible 2-opt swaps
5. **Add fleet management** — Track vehicle counts by type, enforce fleet limits
6. **For multi-problem progression** — Reuse previous problem's solver as baseline, add new constraints incrementally

### Phase 4: Validate and Debug

1. **Run and check for constraint violations** — Verify capacity, time windows, special restrictions are all satisfied
2. **If violations found** — Write targeted fix functions, not universal hacks:
   - Identify violating routes precisely
   - Understand root cause (timing calculation, boundary condition, etc.)
   - Fix the specific issue in cost/check functions
   - Re-run and verify all violations resolved
3. **Compare to baselines** — Check if results are reasonable relative to simpler models

### Phase 5: Analyze Results

1. **Generate structured output** — Save full results as JSON (routes, costs, details per route)
2. **Print human-readable summary** — Route-by-route breakdown with per-route costs
3. **Compute aggregate statistics** — Total cost, distance, CO₂, vehicle counts by type, load rates
4. **Cross-problem comparison** — Compare metrics across sub-problems to identify trends

## Key Patterns

### Data structure pattern
```python
info = {
    'nodes': dict,      # node_id → {kg, m3, cid, tw_start, tw_end, priority}
    'ntw': dict,        # node_id → (tw_start, tw_end)
    'nd': 2D array,     # distance matrix (n+1 × n+1)
    'coords': dict,     # node_id → (x, y)
    'gz': set,          # green zone customer IDs
    'c2n': dict,        # customer_id → [node_ids]
    'agg': dict,        # customer_id → {total_kg, total_m3}
    'nn': list,         # node IDs sorted by distance from depot
}
```

### Cost function pattern
```python
def route_cost(route, vehicle_type, info):
    total = vehicle_type['startup_cost']
    t = 480.0  # start time in minutes from midnight
    has_violation = False
    for each segment (i→j) in route:
        speed = get_speed(t)  # time-dependent
        travel_time = distance / speed * 60
        arrival = t + travel_time
        energy = fpk(speed) * distance / 100 * load_factor
        carbon = energy * emission_factor
        carbon_cost = carbon * carbon_price
        tw_penalty = compute_penalty(arrival, time_window)
        total += energy_cost + carbon_cost + tw_penalty
    return total, has_violation
```

### Greedy insertion pattern
```python
for each customer (sorted by time window start):
    best_cost_increase = INF
    best_vehicle = None
    best_position = None
    for each vehicle:
        for each insertion position in vehicle.route:
            cost_delta = marginal_cost_of_insertion(customer, position)
            if cost_delta < best_cost_increase and constraints_satisfied:
                best = (vehicle, position, cost_delta)
    if best found:
        insert customer into best vehicle at best position
    else:
        create new vehicle for this customer
```

### Violation fix pattern
```python
def find_violations(routes, info):
    violations = []
    for each route:
        simulate arrival times (exactly matching route_cost logic)
        if any constraint violated:
            violations.append(detailed_violation_info)
    return violations

def fix_violations(routes, violations, info):
    for v in violations:
        try: reassign to EV, adjust timing, or re-optimize
        if unfixable: flag for manual review
```

## Sub-problem progression for multi-part problems

When each sub-problem builds on the previous:
- **Problem N** → solve independently
- **Problem N+1** → start from Problem N baseline, add new constraints, run and compare
- **Problem N+2** → start from Problem N+1 baseline, add dynamic event handling, compare to static

Save each problem's results separately, then generate cross-problem comparison analysis.

## Deliverables

For each problem, produce:
1. `problemX_solver.py` — Self-contained solver script
2. `problemX_result.json` — Machine-readable results
3. `problemX_result.txt` — Human-readable summary
4. `figures/*.png` — Visualizations (route maps, cost breakdowns)

Cross-problem:
5. Comprehensive comparison report (DOCX + TXT)
6. Paper template (DOCX)
7. All summary figures
