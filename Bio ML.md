

---

## 🔍 วิเคราะห์แบบละเอียด: “จำเป็นหรือไม่?”

| กรณี                                                          | ควรใส่สมการไหม?          | เหตุผล                                       |
| ------------------------------------------------------------- | ------------------------ | -------------------------------------------- |
| ใช้ ANN, GBR, CatBoost จาก library สำเร็จรูป (ไม่ปรับแกนหลัก) | ❌ ไม่จำเป็น              | เน้นคำอธิบายเชิงฟังก์ชัน + reference เพียงพอ |
| สอนกลไกพื้นฐานให้ผู้อ่านที่ไม่ใช่สาย ML                       | ✅ ใส่ได้ 1–2 สมการง่าย ๆ | เช่น สมการของ perceptron หรือ loss function  |
| ปรับ architecture เอง เช่น activation ใหม่, optimizer ใหม่    | ✅ ควรใส่                 | เพื่อแสดง novelty                            |
| เขียนลง journal ที่เน้น theory-heavy เช่น applied mathematics | ✅ ควรใส่                 | สมการช่วยสร้างความน่าเชื่อถือ                |

---

## ✍️ ตัวอย่างการใส่สมการแบบพอดี ๆ (ถ้าต้องการ)

ถ้าคุณอยากใส่สมการสำหรับ ANN เพื่ออธิบายการคำนวณ output node:

> The output $\hat{y}$ of a fully connected feedforward ANN with one hidden layer can be expressed as:

$$
\hat{y} = f\left(\sum_{j=1}^{n} w_j^{(2)} \cdot \phi\left(\sum_{i=1}^{m} w_{ij}^{(1)} x_i + b_j^{(1)}\right) + b^{(2)}\right)
$$

Where:

* $x_i$: input features
* $w_{ij}^{(1)}$, $w_j^{(2)}$: weights of hidden and output layers
* $b_j^{(1)}$, $b^{(2)}$: biases
* $\phi$: activation function (e.g., ReLU)
* $f$: output activation (e.g., linear or sigmoid)

👆 แบบนี้ **ไม่เยอะเกินไป** และช่วยให้ผู้อ่านสายเทคนิคเข้าใจหลักการเบื้องหลังได้ดีขึ้น


---

## ✅ 1. **Artificial Neural Network (ANN)**

(คุณมีแล้ว แต่ขอใส่ไว้เพื่อความต่อเนื่อง)

$$
\hat{y} = f\left(\sum_{j=1}^{n} w_j^{(2)} \cdot \phi\left(\sum_{i=1}^{m} w_{ij}^{(1)} x_i + b_j^{(1)}\right) + b^{(2)}\right)
$$

Where:

* $x_i$: input features
* $w^{(1)}_{ij}$, $w^{(2)}_j$: weights of hidden and output layers
* $b^{(1)}_j$, $b^{(2)}$: biases
* $\phi$: activation function in hidden layer
* $f$: output activation function (e.g., linear for regression)
* $\hat{y}$: predicted output

---

## ✅ 2. **Gradient Boosting Regression (GBR)**

GBR builds an ensemble model sequentially by fitting each learner to the residuals of the previous ensemble. The model prediction after $M$ iterations is:

$$
\hat{y}(x) = \sum_{m=1}^{M} \gamma_m h_m(x)
$$

Where:

* $h_m(x)$: the weak learner (usually a decision tree) at iteration $m$
* $\gamma_m$: learning rate or step size
* $\hat{y}(x)$: final prediction after $M$ iterations

The model is trained to minimize a differentiable loss function $L(y, \hat{y})$, and at each stage:

$$
r_{i}^{(m)} = - \left[ \frac{\partial L(y_i, \hat{y}_i)}{\partial \hat{y}_i} \right]_{\hat{y}_i = \hat{y}^{(m-1)}_i}
$$

Where $r_{i}^{(m)}$ is the pseudo-residual for sample $i$ at iteration $m$.

---

## ✅ 3. **CatBoost**

CatBoost is also a gradient boosting method, but it improves upon conventional GBR with innovations like **ordered boosting** and **native categorical handling**. Its overall prediction form is:

$$
\hat{y}(x) = \sum_{m=1}^{M} h_m(x; \Theta_m)
$$

Where:

* $h_m(x; \Theta_m)$: tree at iteration $m$ with learned parameters $\Theta_m$
* The training uses **ordered boosting**, which ensures that each prediction is made without information leakage by constructing separate statistics per boosting step.

Additionally, CatBoost applies **target statistics (TS)** for encoding categorical features:

$$
\text{TS}_j = \frac{\sum_{i \in \mathcal{B}_j} y_i + a}{|\mathcal{B}_j| + b}
$$

Where:

* $\mathcal{B}_j$: set of samples with category $j$
* $a$, $b$: smoothing parameters to reduce overfitting

Although CatBoost uses the same loss minimization principle as GBR, its implementation is distinguished by handling categorical features natively and reducing prediction shift.

---

## ✨ เคล็ดลับการใส่สมการใน Section 2.2

คุณสามารถนำสมการเหล่านี้แทรกในคำอธิบายแต่ละโมเดล เช่น:

> The final prediction in GBR is an additive combination of sequentially fitted decision trees:
>
> $$
> \hat{y}(x) = \sum_{m=1}^{M} \gamma_m h_m(x)
> $$
>
> where $h_m(x)$ denotes the base learner at iteration $m$, and $\gamma_m$ is the learning rate.

หรือจะใช้แบบ boxed equation แล้วอธิบายสั้น ๆ ก็ได้ ขึ้นกับรูปแบบของเล่มหรือวารสารครับ

