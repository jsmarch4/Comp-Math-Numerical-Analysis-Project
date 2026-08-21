# Exponentially-Fitted Simpson's Rule for Present Value Estimation

A structure-aware modification of classical Simpson's rule for numerically
integrating present-value cash flow models.

Project for Introduction to Computational Mathematics, with Robby and Luke.

## The idea

Present value is computed by integrating a cash flow stream against a
discount factor:

$$PV = \int_0^T C(t) \cdot e^{-rt} \, dt$$

Standard numerical integration methods (Trapezoidal rule, Simpson's rule)
approximate this integrand using generic polynomial bases, discarding the
fact that every present-value integrand shares the same exponential discount
structure regardless of the cash flow shape $C(t)$.

We modified Simpson's rule to fit a basis that explicitly includes the
discount term itself — $g(t) = c_1 + c_2 t + c_3 e^{-rt}$ — instead of a pure
polynomial. This lets the quadrature rule "know" about the discounting
structure before it even starts approximating.

## Results

Tested against the Trapezoidal rule and standard Simpson's rule across three
cash flow types (exponential, smooth, and oscillatory) at matching panel
counts:

| Cash Flow | Trap Error | Simpson Error | Exp-Fitted Error | Improvement over Simpson |
|---|---|---|---|---|
| Exponential | 5.6×10⁻⁴ | 5.7×10⁻¹⁰ | 5.6×10⁻¹¹ | 10x |
| Smooth | 1.0×10⁻² | 1.5×10⁻⁷ | 7.4×10⁻⁹ | 20x |
| Oscillatory | 2.0×10⁻² | 3.5×10⁻⁴ | 2.1×10⁻⁵ | 16x |

The method also provably recovers standard Simpson's rule exactly as the
discount rate approaches zero, and integrates any function of the form
$c_1 + c_2t + c_3e^{-rt}$ exactly, regardless of step size.

## Contents

- `CMProject_Analysis_of_Trapezoidal_and_Simpsons.ipynb` — full derivation,
  implementation, and convergence analysis (log-log plots across panel
  counts from n = 4 to n = 256).

## Running it

Open the notebook in Jupyter or Google Colab and run all cells. No external
data required — all cash flow test cases are defined directly in the
notebook.
