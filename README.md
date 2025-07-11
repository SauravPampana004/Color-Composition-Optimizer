🎨 Color Composition Optimizer (AI-Powered)
Optimize dye formulations to match target LAB color specifications using machine learning.

📌 Project Overview
In precision dye manufacturing, matching specific color specifications in the CIELAB (L*, a*, b*) space is essential. This project leverages machine learning to develop an intelligent optimizer that predicts which colors and how much of each to add in iterative strokes to minimize color difference (ΔE) and stay within product tolerances.

🧪 Dataset Description
The dataset consists of 11 Excel sheets (S-52 to S-62), each representing a batch scenario in a manufacturing environment.

Each sheet includes:
9 base colors with initial quantities

A target color (master L*, a*, b*)

Initial batch LAB values and calculated differences (ΔL, Δa, Δb, ΔE)

Stroke-wise adjustments: additional color(s) added, resulting LAB values, and ΔE values

A final stroke where ΔE is minimized

Color specification bounds:

L: 21.5 to 22.5

a: –0.6 to –0.2

b: –2.8 to –3.3

🎯 Objective
Build an optimization model that, given the base color quantities and the initial LAB values, recommends the optimal set of color additions (stroke-wise) to bring the batch within the required color spec tolerance and minimize ΔE.

🧠 Approach
1. Data Preprocessing
Read all 11 sheets and consolidate them into a unified DataFrame.

Extract features:

Base color quantities

Initial LAB values, Target LAB

Per-stroke changes in LAB and color additions

Normalize and scale color quantities

2. Feature Engineering
Compute:

Δ(L, a, b) = Target – Initial

Per-stroke LAB delta and ΔE

Construct action vectors representing color added in each stroke

3. Modeling Approaches
✅ Regression-based optimizer (XGBoost + MultiOutputRegressor)
Predict Δ(L, a, b) from current LAB + color additions

🔁 Greedy iterative optimizer
For each stroke, simulate all color additions and choose the one with the lowest predicted ΔE

Optional: Extend to reinforcement learning if the domain complexity increases.

🔄 Optimization Pipeline
Start with initial LAB & target LAB

For each stroke (max 4):

Predict resulting LAB after adding each color

Calculate ΔE

Pick the best color to reduce ΔE

Stop if ΔE is under threshold and LAB is within spec

Sample Code Snippet
python
Copy
Edit
for _ in range(max_strokes):
    best_color, best_lab, best_de = find_best_color(current_lab, target_lab)
    if best_de < 1.0 and within_spec(best_lab):
        break
    strokes.append(best_color)
    current_lab = best_lab
📈 Expected Output
A structured output per batch (sheet):

json
Copy
Edit
{
  "target_lab": [22.0, -0.4, -3.1],
  "initial_lab": [20.5, -0.8, -2.4],
  "predicted_strokes": [
    {"color": "Color 5", "ΔE": 6.1},
    {"color": "Color 3", "ΔE": 3.0},
    {"color": "Color 1", "ΔE": 1.2}
  ],
  "final_lab": [22.1, -0.3, -3.2],
  "final_ΔE": 0.9
}
📦 Tech Stack
Tool/Library	Purpose
Python	Core implementation
Pandas, NumPy	Data manipulation and I/O
XGBoost	Gradient Boosting Regressor
Scikit-learn	Multi-output regression, modeling
Matplotlib/Seaborn	Visualizations
openpyxl	Excel parsing

📌 Challenges Addressed
Matching LAB values across multiple real-world iterations

Predicting additive behavior of colors with nonlinear mixing effects

Working with real dye compositions and tight color tolerance windows

Automating a manual, experience-driven formulation process

🔮 Future Enhancements
Deploy model as an API with a UI where users input initial LAB → get color addition recommendation

Integrate inverse optimization (what mix achieves a target LAB?)

Incorporate physical constraints (e.g., color compatibility, cost limits)

Use GANs or RL for dynamic stroke planning

👨‍🔬 Author
[Saurav Pampana]
AI & ML Enthusiast | Color Science Optimizer
LinkedIn • GitHub

