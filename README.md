# Moving-Least-Squares
# **Summary of Moving Least Squares (MLS) Method**

## **Introduction**
The **Moving Least Squares (MLS) method** is a meshless approximation technique used for function interpolation and numerical analysis. Unlike finite element or finite difference methods, MLS does not require a predefined mesh, making it highly flexible for irregular geometries and adaptive refinement.

## **Key Concepts**
### **1. Approximation Function**
MLS constructs an approximation of a function $$f(x)$$ in the form:

$$f(x) = \sum_{i=1}^{m} p_i(x) a_i(x)$$

where:
- $$p_i(x)$$ are basis functions (e.g., monomials from a Pascal triangle expansion).
- $$a_i(x)$$ are coefficients that minimize a weighted least squares error.

### **2. Pascal Triangle Basis Functions**
The basis functions are selected from a **2D Pascal Triangle** up to second-order terms:

$$
p(x, y) = \begin{bmatrix} 1, & x - x_0, & y - y_0, & (x - x_0)^2, & (x - x_0)(y - y_0), & (y - y_0)^2 \end{bmatrix}^T
$$

This basis includes:
- **Constant term**: $1$
- **Linear terms**: $x - x_0$, $y - y_0$
- **Quadratic terms**: $(x - x_0)^2$, $(x - x_0)(y - y_0)$, $(y - y_0)^2$

These polynomials ensure local smoothness and accuracy in function approximation.

### **3. Weighting Function**
A weight function $w(x)$ is introduced to ensure that nearby points influence the approximation more than distant points. A common choice is the **Gaussian weight function**:

$$
w(x) = e^{-\left(\frac{|x - x_i|}{d_i}\right)^2}
$$

where $d_i$ is a characteristic distance defining the influence zone.

In the code, **30 nearest neighbors** are used for each evaluation point. The radius of influence is determined dynamically using the distance to the **30th nearest neighbor**.

### **4. Normal Equation System**
The MLS method determines the unknown coefficients $a_i(x)$ by solving:

$$
A(x) a(x) = B(x) f
$$

where:
- $A(x)$ is the **moment matrix**, formed from basis functions and weight functions.
- $B(x)$ is a matrix related to the weight function and sample points.
- $f$ is the vector of function values at known sample points.

### **5. Derivatives Computation**
Once the **shape functions** are computed, derivatives (gradients, Hessians) can be found using:

$$
\frac{\partial f}{\partial x} = \sum_{i=1}^{m} \frac{\partial p_i(x)}{\partial x} a_i(x)
$$

This allows MLS to be used for solving **partial differential equations (PDEs)**.

## **Implementation Details**
- Our **Python implementation** uses **30 nearest neighbors** for each evaluation point.
- The **moment matrix** is regularized using the **pseudo-inverse** (`pinv`) to handle potential singularities.
- **Shape functions** and their derivatives are computed and stored as sparse matrices.
- The condition number of $A(x)$ is recorded to monitor numerical stability.

## **Advantages of MLS**
✅ **Meshless**: No need for predefined connectivity between points.  
✅ **High Accuracy**: Smooth and continuous function representation.  
✅ **Adaptability**: Can handle complex geometries and irregular point distributions.  
✅ **Locality Control**: Influence is restricted to a fixed number of nearest neighbors (30 in this case).  

## **Applications**
🔹 Computational Fluid Dynamics (CFD)  
🔹 Structural Analysis (Meshfree methods like SPH)  
🔹 Image Processing (Interpolation, Denoising)  
🔹 Machine Learning (Kernel-Based Regression)  

---
**References:**  
- Liu, G.R. (2003). Meshfree Methods: Moving Beyond the Finite Element Method. *CRC Press*.

  ## **Code Availability**

  
⚠️ **This method has been used in the following research paper:**  
➡ **Boutopoulos, Ioannis D., et al. "Two-phase biofluid flow model for magnetic drug targeting." Symmetry 12.7 (2020): 1083.**  

📌 **If you want access to the implementation codes from the paper, please let me know.** 
