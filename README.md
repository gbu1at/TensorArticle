# Тензоры и тензорное произведение

Главная цель данного отчета — рассказать о тензорах и их применении в алгебре.

## **Введение**

Все началось с того, что я наткнулся на [видео на YouTube](https://m.youtube.com/watch?v=fDAPJ7rvcUw) 2022 года, в котором говорилось о том, что **нейронная сеть** смогла умножить матрицу 4×4 быстрее, чем по **алгоритму Штрассена**.

Оценка времени выполнения алгоритмов умножения матриц зависит от их **асимптотической сложности** и **конкретной реализации**. Давайте сравним **AlphaTensor** и **алгоритм Штрассена** для умножения матриц в поле *Z<sub>2</sub>*.

---

### 1. **Алгоритм Штрассена**
Алгоритм Штрассена — это классический алгоритм для умножения матриц, который уменьшает количество операций умножения за счёт рекурсивного разбиения матриц на блоки. Подробнее о нем можно прочитать [здесь](https://ru.wikipedia.org/wiki/Алгоритм_Штрассена).

#### Асимптотическая сложность:
- Для матриц размера $n \times n$ алгоритм Штрассена имеет сложность:

$$
O(n^{\log_2 7}) \approx O(n^{2.81}).
$$

#### Количество операций:
- Для матриц 2×2 алгоритм Штрассена требует **7 умножений**.
- Для матриц 4×4 требуется **49 умножений** (7 умножений для каждого из 7 подблоков 2×2).

---

### 2. **AlphaTensor**
AlphaTensor использует нейронные сети для поиска более эффективных алгоритмов умножения матриц. В частности, он может находить разложения, которые требуют меньше операций умножения, чем алгоритм Штрассена.

#### Количество операций:
- Для матриц 4×4 AlphaTensor может найти разложение, которое требует **47 умножений** вместо 49.

---

### Итог
- **Алгоритм Штрассена**: Асимптотическая сложность $O(n^{2.81})$, 7 умножений для матриц 2×2, 49 умножений для матриц 4×4.
- **AlphaTensor**: Асимптотическая сложность $O(n^{2.78})$.

Плюс ко всему **AlphaTensor** может не только уменьшить количество умножений, но и сократить количество сложений! Внутри себя **AlphaTensor** использует понятие тензора и тензорного произведения.

---

## **Тензоры**

Давайте разбираться, что же такое **тензоры**. Это может быть страшно и непонятно, но в целом вещь нужная и полезная для понимания алгебры. Чтобы понять, что это такое, мне показался очень качественным и небольшим курс у [этого парня](https://m.youtube.com/watch?v=8ptMTLzV4-I&list=PLJHszsWbB6hrkmmq57lX8BV-o-YIOFsiG&index=1&t=35s&pp=iAQB). Он начинается прямо с нуля, и даже была возможность немного вспомнить азы линейной алгебры.

А также [видео этого мужичка](https://m.youtube.com/watch?v=K7f2pCQ3p3U) мне тоже очень помогло понять более конкретно и, главное, формальное определение **тензорного произведения** для общего случая.

---

### **Мотивация**

Вообще, мы знаем достаточно много всяких разных конструкций: *линейный функционал*, *линейное отображение*, *билинейное отображение*, *полилинейное отображение* и т.д. Их несчетное множество, и для каждого отображения формулировать леммы, теоремы и доказывать различные факты нет возможности. Еще немаловажно понимать структуру **базиса**, а точнее, какое преобразование происходит при его смене.

Тем более, что для каждого отображения у нас может меняться как поле, так и **векторное пространство**. Например, чтобы научиться умножать матрицы 2×2, нам приходится рассматривать векторное пространство этих матриц.

### **Тензоры. Начало**

Давайте дадим формальное определение **тензору** и **тензорному произведению**.

![](/img/image2.png)

Вот смотрите, мы вводим некоторое пространство и обозначаем его *U* ⊗ *V*. Этой записи пугаться вообще не нужно. Главное держать в голове, что *U* ⊗ *V* — это просто какое-то пространство с операцией сложения и умножения на элемент нашего поля. И главное его свойство в том, что, во-первых, оно существует для любых конечномерных *U* и *V*, а во-вторых, у него есть очень *"прикольное"* свойство.

В общем случае, как такое пространство задать, можно посмотреть [тут](https://m.youtube.com/watch?), но у меня нет цели вдаваться в такие подробности. Чуть дальше мы на конкретном примере построим наш тензор. Так, окей, с этой записью разобрались: *U* ⊗ *V*. Чуть позже постараемся понять, при чем тут **билинейное отображение** и зачем оно нам так важно?

### **Построение Базиса**

Для начала я хочу рассмотреть некоторые свойства нашего полученного пространства. Давайте в пространстве *V* выделим базис:

**e<sup>1</sup>, e<sup>2</sup>, ..., e<sup>n</sup>**

и базис для *U*:

**f<sub>1</sub>, f<sub>2</sub>, ..., f<sub>m</sub>**

Тогда базисом нашего тензора будут элементы:

**e<sup>i</sup> ⊗ f<sub>j</sub>**

Этого тоже пугаться не нужно. Хоть я и опущу доказательство этого факта, но важно понимать, что это так. У нас есть пространство *V* ⊗ *U*. У этого пространства в любом случае есть базис. Но из определения данного пространства мы фактически бесплатно получаем его базис. Это **e<sup>i</sup> ⊗ f<sub>j</sub>**.

---

### *Параграф на понимание (не обязательный для дальнейшего прочтения)*

Сейчас я приведу небольшой пример на понимание. Он немного абстрактный, но мне кажется, в этом и есть его плюс.

Зададимся целью определить **тензорное произведение линейных отображений**. Множество линейных отображений {L: *U<sub>1</sub>* → *V<sub>1</sub>*} и {S: *U<sub>2</sub>* → *V<sub>2</sub>*} — это какие-то пространства с базисами. Значит, можно какой-нибудь тензор построить.

![](/img/image3.png)

Вроде бы похоже на какой-то обман. Ну, якобы, чтобы определить тензорное произведение линейных отображений, нужно знать тензорное произведение пространств для них. Но с другой стороны, если мы умеем строить тензорное произведение пространств, то мы бесплатно получаем структуру тензорного произведения линейных отображений. А это очень даже приятная новость.

Доказательство этого факта опустим. Тут важно приметить, что мы явно это тензорное отображение задали, а не просто сказали, что оно существует.

Ну и в добавок факт на понимание, который напрямую следует из определения выше:

![](/img/image4.png)

---

Я обещал пример какого-нибудь тензора. Давайте как раз его сейчас построим, но для этого (и в дальнейшем) мне просто необходимо ввести понятие *V<sup>*</sup>*.

---

### **Определение V<sup>*</sup>**

*V<sup>*</sup>* — это **двойственное пространство** (или **сопряжённое пространство**) к векторному пространству *V*. Давайте разберёмся, что это означает.

---

#### 1. **Определение**
Двойственное пространство *V<sup>*</sup>* — это множество всех **линейных функционалов** на *V*. *Линейный функционал* — это линейное отображение из *V* в поле *K* (обычно *ℝ* или *ℂ*):

$$
f: V \to K
$$

которое удовлетворяет условию линейности:

$$
f(a \cdot u + b \cdot v) = a \cdot f(u) + b \cdot f(v),
$$

где *u, v ∈ V*, *a, b ∈ K*.

---

#### 2. **Базис двойственного пространства**
Если *V* — конечномерное пространство с базисом *e<sub>1</sub>, e<sub>2</sub>, ..., e<sub>n</sub>*, то двойственное пространство *V<sup>*</sup>* имеет **двойственный базис** *e<sup>1</sup>, e<sup>2</sup>, ..., e<sup>n</sup>*, где:

$$
e^i(e_j) = \delta^i_j,
$$

и *δ<sup>i</sup><sub>j</sub>* — символ Кронекера (равен 1, если *i = j*, и 0 иначе).

#### 3. **Пример**
Предположим, что *V* — это *n*-мерные векторы-столбцы. Тогда *V<sup>*</sup>* — это *n*-мерные векторы-строки, действующие на элементы из *V* посредством умножения. То есть вектор-строка умножается на вектор-столбец, и мы получаем как раз скаляр.

С этим очень удобно работать, и мы будем использовать это далее очень часто.

---

### **Тензоры. Пример Тензорного произведения**

Явно построим элементы *тензорного произведения* *V* ⊗ *V<sup>*</sup>*. Для этого перемножим столбец матрицы на строку:

$$
\begin{pmatrix} a_1 \\ a_2 \\ a_3 \\ a_4 \end{pmatrix} \begin{pmatrix} b_1 & b_2 & b_3 & b_4 \end{pmatrix} = \begin{pmatrix}
a_1 b_1 & a_1 b_2 & a_1 b_3 & a_1 b_4 \\
a_2 b_1 & a_2 b_2 & a_2 b_3 & a_2 b_4 \\
a_3 b_1 & a_3 b_2 & a_3 b_3 & a_3 b_4 \\
a_4 b_1 & a_4 b_2 & a_4 b_3 & a_4 b_4
\end{pmatrix}
$$

Рассмотрим базис:

$$
\begin{pmatrix} 0 \\ 0 \\ 1 \\ 0 \end{pmatrix} \begin{pmatrix} 0 & 1 & 0 & 0 \end{pmatrix} = \begin{pmatrix}
0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 \\
\end{pmatrix}
$$

Согласитесь, не самое занимательное явно построенное тензорное произведение.

### **Тензоры. Продолжение. Важные свойства**

Наверное, далее что хотелось бы сказать, это то, что

*V* ≅ *V<sup>**</sup>*

это тоже какая-то задача, особо на ней не хочу заострять внимание, но проговорить его нужно. Также хочу напомнить о том, что означает этот значок ≅ — это **изоморфизм**. То есть между *V* и *V<sup>**</sup>* существует биективное линейное отображение.

Давайте рассмотрим очень важное замечание:

![](/img/image5.png)

Это на самом деле обобщение тензорного произведения более чем на 2 пространства. В данном случае на *n* (там на картинке *k*, но это опечатка).

Если мы в качестве *n* возьмем 2, то получим изначальное определение тензорного произведения.

И наверное нам придется еще немного просмотреть некоторые теоремы, но с опущенным доказательством ":(".

![](/img/image6.png)

1) Это **ассоциативность** тензорного произведения.
2) Это **коммутативность** тензорного произведения.
3) Это очень важный факт. То есть множество линейных операторов изоморфно множеству тензорного произведения. Доказательство этого факта не очень сложное, но если подумать глобально, что мы смогли получить и подставить множество гомоморфизмов пространств в определение тензорного произведения, наблюдается невероятно интересная связь между билинейной формой, множеством гомоморфизмов и линейным отображением. (Я просто подставил множество линейных операторов в определение тензора, и меня это очень зацепило, не знаю, может для вас данное следствие очевидно :) ).
4) Тоже полезный факт, будем пользоваться им в дальнейшем.

### **Как Тензоры связаны с нашей задачей перемножения матриц?**

Хорошо, теперь давайте немного приблизимся к нашей задаче. Рассмотрим **билинейное отображение** *f: V × V → V*. Это отображение как раз описывает нам произведение матриц. В нашей задаче в качестве *V* мы будем рассматривать матрицы 4×4. Они тоже образуют векторное пространство.

Если мы вспомним определение тензора, то мы получим следующий факт: для любого билинейного отображения *f: V × V → V* существует линейное отображение *g: V ⊗ V → V*.

Хм, ну линейное отображение тоже можно записать в виде тензора. (Помните, выше была картинка с пятью фактами про тензоры?)

{*g: V ⊗ V → V*} ≅ (*V ⊗ V*)<sup>*</sup> ⊗ *V* ≅ *V<sup>*</sup>* ⊗ *V<sup>*</sup>* ⊗ *V*

Супер! Осталось ввести простое и интуитивно понятное определение:

### **Ранг Тензора**

![](/img/image8.png)

## **Финальное следствие**

У нас все есть, чтобы привести финальное следствие :)

![](/img/image7.png)

Заметим, что f<sup>i</sup>(A) и g<sup>i</sup>(B) - это линейные выражения от коэффициентов матриц, поэтому они могут быть
вычислены при помощи сложения чисел и умножения чисел на фиксированные скаляры
для того, чтобы перемножить f<sup>i</sup>(A) и  g<sup>i</sup>(B) нужны полноценные операции произведения. Тут и происходит рекурсивный вызов
Так как коэффициенты 𝑣𝑖 постоянны, то для этой операции тоже не
нужно честное умножение. Итого, реально используется 𝑟 настоящих умножений.

Покажем явное разложение для алгоритма Штрассена

--- 

$$
A = \begin{pmatrix} 
A_{1,1} & A_{1,2} \\ 
A_{2,1} & A_{2,2} 
\end{pmatrix}, \quad 
B = \begin{pmatrix} 
B_{1,1} & B_{1,2} \\ 
B_{2,1} & B_{2,2} 
\end{pmatrix}.
$$

--- 

$$
f_1 = A_{1,1} + A_{2,2} \quad g_1 = B_{1,1} + B_{2,2} \quad P_1 = f_1 \cdot g_1
$$

$$
f_2 = A_{2,1} + A_{2,2} \quad g_2 = B_{1,1} \quad P_2 = f_2 \cdot g_2
$$

$$
f_3 = A_{1,1} \quad g_3 = B_{1,2} - B_{2,2} \quad P_3 = f_3 \cdot g_3
$$

$$
f_4 = A_{2,2} \quad g_4 = B_{2,1} - B_{1,1} \quad P_4 = f_4 \cdot g_4
$$

$$
f_5 = A_{1,1} + A_{1,2} \quad g_5 = B_{2,2} \quad P_5 = f_5 \cdot g_5
$$

$$
f_6 = A_{2,1} - A_{1,1} \quad g_6 = B_{1,1} + B_{1,2} \quad P_6 = f_6 \cdot g_6
$$

$$
f_7 = A_{1,2} - A_{2,2} \quad g_7 = B_{2,1} + B_{2,2} \quad P_7 = f_7 \cdot g_7
$$

$$
v_1 = \begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}, \quad v_2 = \begin{pmatrix} 0 & 0 \\ 0 & -1 \end{pmatrix}, \quad v_3 = \begin{pmatrix} 0 & 1 \\ 0 & -1 \end{pmatrix}, \quad v_4 = \begin{pmatrix} 1 & 0 \\ 1 & 0 \end{pmatrix},
$$

$$
v_5 = \begin{pmatrix} -1 & 1 \\ 0 & 0 \end{pmatrix}, \quad v_6 = \begin{pmatrix} 0 & 0 \\ 0 & 1 \end{pmatrix}, \quad v_7 = \begin{pmatrix} 1 & 0 \\ 0 & 0 \end{pmatrix},
$$

--- 

Элементы $f_i, g_i, v_i$ задают разложение тензора матричного умножения. Если $C = AB$, то для элементов $C$ справедливы формулы:

$$
C_{1,1} = P_1 + P_4 - P_5 + P_7
$$

$$
C_{1,2} = P_3 + P_5
$$

$$
C_{2,1} = P_2 + P_4
$$

$$
C_{2,2} = P_1 - P_2 + P_3 + P_6
$$

## Заключение 
Если вы хотите посмтотреть веса для разложение мтариц 4 на 4, то вот официальный репозиторий

https://github.com/google-deepmind/alphatensor

и официальная публикация
https://www.nature.com/articles/s41586-022-05172-4

на самом деле **AlphfTensor** нашел ускороене для произведений матриц не только размера 4 на 4: 4х5 на 5х5, 11х12 на 12х12 и тд 

Но идея данного поста была рассказать про тензоры и их значимость и применение в алгебре

--- 

