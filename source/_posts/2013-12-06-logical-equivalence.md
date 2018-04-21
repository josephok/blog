---
layout: post
title: '逻辑等价'
date: 2013-12-06 06:42
comments: true
categories: Math
---
如果p ↔ q是永真式，命题p和q称为是逻辑等价的。记号p≡q表示p和q逻辑等价。

* 逻辑等价公式

1. Identity laws 恒等律：
> p∧T≡p
p∨F≡P

2. Domination laws 支配律：
> p∨T≡T
p∧F≡F

3. Idempotent laws 幂等律:
> p∨p≡p
p∧p≡p

4. Double negation laws 双非律:
> ﹁(﹁p)≡p

5. Commutative laws 交换律:
> p∨q≡q∨p
p∧q≡q∧p

6. Assocative laws 结合律:
> (p∨q)∨r≡p∨(q∨r)
(p∧q)∧r≡p∧(q∧r)

7. Distributive laws 分配律:
> p∨(q∧r)≡(p∨q)∧(p∨r)
p∧(q∧r)≡(p∧q)∨(p∧r)

8. De Morgan's laws 德摩根律:
> ﹁(p∧q)≡﹁p∨﹁q
﹁(p∨q)≡﹁p∧﹁q

9. Absorption laws 吸收律:
> p∨(p∧q)≡p
p∧(p∨q)≡p

10. Negation laws 否定律:
> p∨﹁p≡T
p∧﹁p≡F

* 包括蕴涵的逻辑等价：

1. p→q≡﹁p∨q
2. p→q≡﹁q→﹁p
3. p∨q≡﹁p→q
4. p∧q≡﹁(p→﹁q)
5. ﹁(p→q)≡p∧﹁q
6. (p→q)∧(p→r)≡p→(q∧r)
7. (p→q)∨(p→r)≡p→(q∨r)
8. (p→r)∧(q→r)≡(p∧q)→r
9. (p→r)∨(q→r)≡(p∨q)→r

* 包含双蕴涵（逻辑双条件）的逻辑等价：

1. p↔q≡(p→q)∧(q→p)
2. p↔q≡﹁p↔﹁q
3. p↔q≡(p∧q)∨(﹁p∧﹁q)
4. ﹁(p↔q)≡p↔﹁q

### 证明(p∧q)→q为永真式:
证明：(p∧q)→q ⇔ ﹁(p∧q)∨q
				     ⇔ (﹁p∨﹁q)∨q (根据德摩根律)
             ⇔ ﹁p∨(﹁q∨q) (根据结合律)
             ⇔ ﹁p∨T (根据否定律)
             ⇔ T (根据支配律)
由此得证。
