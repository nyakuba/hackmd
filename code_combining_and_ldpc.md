# Длинные коды из коротких кодов
###### tags: `git`

## Методы комбинирования кодов
### Код-произведение

:::info
Код-произведение задается парой кодов $C_1$, $C_2$ с параметрами $(n_1, k_1, d_1)$, $(n_2, k_2, d_2)$ соответственно.

- Вектор данных размера $k_1 \times k_2$ нарежем на блоки по $k_1$ символов и закодируем их кодом $C_1$. 
- Полученные $k_2$ кодовых слов кода $C_1$ запишем в матрицу $k_2 \times n_1$ по строкам. 
- Столбцы матрицы закодируем кодом $C_2$. Полученный вектор размера $n_1 \times n_2$ --- кодовое слово кода-произведения.
:::

Пусть коды $C_1, C_2$ заданы порождающими матрицами $G^{(1)}, G^{(2)}$ соответственно. Как найти порождающую матрицу кода произведения?
\begin{equation}
  G^{(1)} = \begin{pmatrix}
    101\\
    011\\
  \end{pmatrix}, \quad G^{(2)} = \begin{pmatrix}
    1111\\
    1010\\
    1100\\
  \end{pmatrix}
\end{equation}

:::info
Порождающая матрица кода-произведения --- кронекеровское произведение порождающих матриц $G_1$ и $G_2$:
\begin{equation}
  G = G^{(1)} \otimes G^{(2)} = \begin{pmatrix}
  G_{11}^{(1)}G^{(2)} & \dots & G_{1n_1}^{(1)}G^{(2)}\\
    \vdots & \ddots & \vdots\\
    G_{k_11}^{(1)}G^{(2)} & \dots & G_{k_1n_1}^{(1)}G^{(2)}\\
  \end{pmatrix}
\end{equation}
:::

Чему равно минимальное расстояние кода из предыдущего примера?
\begin{equation}
  n = n_1n_2 = 12, \quad k = k_1k_2 = 6, \quad d = d_1d_2 = 4
\end{equation}
    
Получили код с оптимальными параметрами.

Обобщение данной конструкции --- каскадные и обобщенные каскадные (многоуровневые) коды.

### Конструкция Плоткина.

:::info
Пусть заданы коды $C_1$, $C_2$ с параметрами $(n_1, k_1, d_1)$, $(n_2, k_2, d_2)$ соответственно.

Код $C$ получаемый конструкцией Плоткина содержит вектора вида
\begin{equation}
  C = \{ (u, u+v) \mid u \in C_1, v \in C_2 \}.
\end{equation}
:::

Пусть $G^{(1)}, G^{(2)}$ --- порождающие матрицы кодов $C_1, C_2$ соответственно. Запишите порождающую матрицу кода $C$.

:::info
Полученный код $C$ имеет параметры $n = n_1 + n_2, k = k_1 + k_2, d = \min\{2d_1, d_2\}$.
:::
Докажите это утверждение.

:::info
Пример кодов --- коды Рида-Маллера, полярные коды.
:::

## Коды с малой плотностью проверок на четность

:::info
Пусть дана базовая матрица и матрица степеней
\begin{equation}
  B = \begin{pmatrix}
    1 & 1 & 1 & 1\\
    1 & 1 & 1 & 1\\
    1 & 1 & 1 & 1\\
  \end{pmatrix}, \quad W = \begin{pmatrix}
    0 & 0 & 0 & 0\\
    0 & 1 & 0 & 1\\
    1 & 2 & 1 & 2\\
  \end{pmatrix}.
\end{equation}
:::

Постройте проверочную матрицу кода длины 16.

---

:::info
Дана базовая матрица и матрица степеней
\begin{equation}
  B = \begin{pmatrix}
    1 & 1 & 1\\
    1 & 1 & 1\\
  \end{pmatrix}, \quad W = \begin{pmatrix}
    0 & 0 & 0\\
    0 & 1 & 1\\
  \end{pmatrix}.
\end{equation}
:::

Запишите проверочную матрицу и постройте граф Таннера кода длины 6.

Восстановите стирания в принятом векторе
\begin{equation}
  y = (0, 0, \epsilon, 1, \epsilon, 1).
\end{equation}

:::info
В канале с АБГШ с двоичной модуляцией для каждого символа вычисляется логарифмическое отношение правдоподобия (ЛОП)
\begin{equation}
L(y) = \frac{W(y | - 1)}{W(y | +1)} = \frac{2y}{\sigma^2}.
\end{equation}

Для декодирования применяется алгоритм распространения доверия.
Введем обозначения:
- $C(s)$ ---множество проверочных узлов инцидентных символьному узлу $s$
- $S(c)$ --- множество символьных узлов инцидентных проверочному узлу $c$
- $L_s$ --- ЛОП символа $s$, полученный из канала
- $L_{cs}$ --- ЛОП ребра $(c, s)$

\begin{align}
  &L_{cs} = L_s + \sum_{c' \in C(s) \setminus \{c\}} L_{c's},\\
  &L_{cs} = \left[ \prod_{s' \in S(c) \setminus \{s\}} {\rm sign}(L_{cs'}) \right] \phi \left( \sum_{s' \in S(c) \setminus \{s\}} \phi(L_{cs'})\right),\\
  &\phi(x) = -\ln\tanh(x/2)
\end{align}

От необходимости вычисления функции $\phi(x)$ можно отказаться путем использования аппроксимации:
\begin{equation}
  L_{cs} = \left[ \prod_{s' \in S(c) \setminus \{s\}} {\rm sign}(L_{cs'}) \right] \min_{s' \in S(c) \setminus \{s\}} |L_{cs'}|.
\end{equation}
:::

Пусть на выходе канала был получен вектор ЛОПов
\begin{equation}
  y = (-5, -5, 1, -1, -2, -3).
\end{equation}

Выполните декодирование.