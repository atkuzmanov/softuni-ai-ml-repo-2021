﻿Dimensionality Reduction
* използва се за намаляване на колонките в dataset (reducing the number of features via projection)
* curse of dimensionality
	* много колонки не оказват никакво влияние или оказват, но е много малко
	* или имаме "едни и същи" данни в различни колонки
	* колкото повече са колонките, толкова повече нашия алгоритъм се забавя
* основната идея е да махнем ненужните колонки и да забързаме нашите модели
* ако имаме много на брой колонки но те са независими една от друга, това не пречи на модела, но ако са зависими му пречи
* проблем е ако имаме повече на брой колонки от колкото наблюдения (редове)
	* повечето алгоритми не могат да работят правилно в този случай
* да махаме дадени колонки е Feature Selection
* Principal Component Analysis (PCA) - Анализ на първичните компоненти
	* Стандартен алгоритъм
	* Търсим комбинации от колонки които дават добра информация
	* A technique for feature extraction
		* Transforms all variables so they are **linearly independent**
			* This is a linear transformation
		* Помага когато имаме корелация в данните (т.е. една променлива да зависи от друга)
		* Връща данните като линейно независими
		* Ако имаме колонка която е "функция" от друга/и, може да махнем някои колонки за да нямаме повтарящи се данни
	* Всяко едно измерение е една колонка
	* The transformation is done to increase the explained variance in the data
		* идеята е че имаме данни които са разположени в кординати и ние трябва да сменим координатите по някакъв начин
			* може да намерим координатите които обясняват най-добре данните
	* Number of Principal Components (PC): **p = min(m,n)** (Брой първични компоненти)
		* m - number of features
		* n - number of observations
		* Sorted from highest to lowest explained variance
	* Most widely used practical application
		* Load a high-dimensional dataset with "m" features
		* Perform PCA, remove the last "k" components
			* Result: dataset with "m - k" features
			* These **are not** "m - k" of the original columns!
	* Имаме загуба на данни, обаче имаме по-малко колонки и по-голяма бързина в модела
		* това ще ни позволи да тренираме повече и да използваме повече "хипер параметри"
	* Интуиция за PCA
		* PCA търси трансформацията която дава най-голямата вариация (проекцията която дава най-голяма вариация в данните)
			* вариация - колко е разликата между минимума и максимума???
		* Първичните компоненти винаги са перпендикулярни една спрямо друга
		* Пример: сенки (тримерен обект има двумерна сянка)
			* ако имаме една лампа и един обект, може да видим каква сянка има обекта спрямо лампата
			* идеята на PCA е да намери това осветяване на обекта което дава най-подробна информация
				* сянката да даде най-много информация за обекта
	* **Когато използваме PCA трябва да скалираме данните предварително!**
		* в противен случай, компонентите ще бъдат колонките с по-големите числа, а не колонките които дават най-добра информация
	* PCA е unsupervised метод
	* **pca.explained_variance_ratio** - Percentage of variance explained by each of the selected components
	* Всяка една първична компонента е линейна комбинация от оригиналните features (колонки)
		* т.е. сбора на всички features умножени с някакъв коефициент
			* всяка една колонка е умножена по някакъв коефициент
		* Как можем да възстановим от компонентите оригиналните коефициенти и защо това може да ни бъде полезно?
			* ако можем да ги възстановим, можем да разберем от първата компонента коя или кои са най-важните колонки
	* Съветва се да не се правят интерпретации на PCA резултати
	* Ако имаме много на брой измерения първите компоненти ни дават най-много вариация
		* може да плотнем първите 2-3 
			* ако има някаква структура в тях, може да ни дадат информация за реалната структура в много измерения
			* Ползва се за визуализация на данни, освен за намаляване на размерността
	* Математиката за PCA
		* Точките си остават същите, но начина по който ние гледаме на тях се променя
		* Има трансформации които не променят центъра на координатната система
			* тези трансформации могат само да разтеглят, свиват, завъртат и т.н., но не променят центъра на координатната система
			* наричат се линейни трансформации
				* пример: shear transformation
		* Всяка една трансформация може да се характеризира със **матрица на трансформацията**
		* Има вектори по които като умножим матрицата действат като умножение с число (сменят само големината)
			* т.е. не си променят посоките, а само големините
			* Ако предположим че знаем дадена матрица, може да потърсим за нея такива вектори
		* Eigenvalues - собствени стойности на матрицата
		* Eigenvectors - собствени вектори на матрицата
		* Можем да намерим тези Eigenvalues и Eigenvectors
		* Какво прави PCA?
			1. Намира кои са векторите (Eigenvectors)
				* това са новите първични компоненти
			2. Разбира за всеки един вектор, колко оригинална вариация съдържа
			3. Сортира векторите по намаляващ ред (от най-важни към най-маловажни)
			4. Връща като резултат сортираните вектори
			* Нарича се още "разлагане" на матрицата
	* Работи само за линейни трансформации
		* Ами ако някъде имаме трансформация която не е линейна?
			* ползваме kernels
	* Kernel PCA
		* Ако линейна трансформация не ни свърши работа може да използваме kernels компонентите
			* Използват се както при SVMs
			* Една функция която вкарваме в трансформацията
		* Имаме същите видове kernels както при SVMs (rbf (gaussian), sigmoid, polynomial, linear)
		* This allows us to separate non-convex and non-linear data
			* Можем да връщаме с тях не-линейни трансформации (non-linear)
			* Също така работи и за неизпъкнали данни (non-convex)
				* Пример за неизпъкнали: moons
				* Пример за изпъкнали: blobs
			* Example: nested circles
			* We need to set the gamma parameter
				* width of the kernel (in case of "rbf" kernel)
				* if we don't know the exact value, we need to perform grid search
	* Ако нямаме рязко спадане на вариацията, вероятно сме сбъркали нещо или данните не се подават на такава интерпретация
		* данните вече може да имат ниска размерност и да не могат да бъдат намалени още повече
	* Може да се изследва отделно всеки брой първични компоненти с реални данни (да го тестваме с реални данни)
		* т.е. да fit-нем друг алгоритъм със първите 2 компоненти, първите 8 компоненти и първите 15 компоненти и да видим резулаттите при hold-out set
		* т.е. при testing data как ще изглеждат резултатите за различен брой компоненти
	* Обикновено PCA се използва като стъпка преди да използваме друг алгоритъм
* Linear Discriminant Analysis (LDA or LinDA)
	* **Supervised method** which tries to identify the attributes that account for the most variance classes
		* Used in **classification** cases, with known class labels
		* Returns a transformation of the input data, similar to PCA
	* **Linear Discriminant Analysis** and **Latent Dirichlet Allocation** have the **same acronyms**, and are both used for dimensionality reduction / transformation
		* They **work in** completely **different ways**
	* Връща нови кординати (трансформация) които работят подобно на PCA, но максимизират вариацията във всяка една компонента (координата) спрямо отделните класове
	* Използва се **само за класификация**
	* Използваме линейна граница
		* Опитваме се да максимизираме отделните групи ако можем да ги разделим с прави линии
	* Начин да правим търсене на групи, нещо подобно на Analysis of Variance (ANOVA)
		* подобно на clusters, имаме групички (в случая по класове) в които вариацията в тях е малка, но между една група и друга група е голяма

Manifold Learning
* manifold - повърхност
* "Flattening out" surfaces in fewer dimensions
	* целта му е да търси повърхности и да ги "изправя"
* Идеята е да можем да видим реалната структура, която е скрита и е изкуствено в много на брой измерения
* Искаме да намерим структура която е свързана с близки съседи в по-малко мерното пространство
* Предположението е че имаме една фигура (в общия случай) която е нагъната или навита
* Преположението е че броят на измеренията е изкуствено увеличен
* Idea
	* The dimensions are really high
	* Even though there are many features, they can be expressed with a few parameters
		* "Low-dimensional manifold in a high-dimensional space"
		* Non-linear methods
* Example: "Swiss roll" dataset
	* 2D "curled" shape in 3D
* Used for dimensionality reduction. Also, it's a common way to visualize high-dimensional datasets
	* Especially for **classification cases**
* Най често използвания метод е **Isometric Mapping** (Isomap in sklearn)
	* Search a lower-dimensional projection (embedding) which keeps the distances between the points
		* Търсим такава проекция която пази разстоянията между всяка една точка
	* Algorithm overview
		* Find the "k" nearest neighbors of each point (KNN)
		* Construct a connectivity graph
			* 2 points are connected if they're neighbors (from the 1st step)
		* Compute shortest paths (Dijkistra, Floyd-Warshall)
		* Perform projection on the graph
			* Similar to PCA
	* Основно приложение: работа върху данни от карта
* Между clustering и manifolds има много голяма връзка
	* clustering търси купчинки
	* manifolds търси форми на които трябва да намали размерността
	* често се използват заедно
* t-SNE (t-distributed Stochastic Neighbor Embedding)
	* Isomap tries to "unfold" a continuos structure
	* t-SNE looks for local cluster in the data
		* useful for revealing clusters and structure
	* Usual implementation: Barnes - Hut (2D or 3D only)
		* To preserve high-dimensional structure, we need to initialize it with PCA (instead of randomly)
		* It can be slow
		* Mainly used for visualization in image and text processing
	* Търсим близки съседи на "случаен" принцип
	* Удобно за комбинация от клъстериране и намаляване на размерността
	* Търси така да намали размерността, че скупчванията от точки да останат близки
	* Прави нещо подобно на клъстериране, само че стохастични методи (случайни съседи) - случаен subset от близки съседи
	* Много е важно да имаме една и съща инициализация, подобно на k-means, така и този алгоритъм може да намери различни съседи, ако не искаме това да се случва трябва да дадем инициализация с PCA (init = "pca")
	* Може да се използва при text processing, например да търсим различни видове теми от много статии (една статия може да бъде за програмиране, спорт, почивка и т.н.)
	* При многомерни datasets може да визуализираме дали има някаква структура по-малко мерно пространство
		* ако има структура в по-малко мерното пространство, то не знаем дали има структура и в повече мерни пространства
		* ако няма структура в по-малко мерното пространство, то със сигурност няма и в повече мерни пространства

* Real-World Examples for Dimensionality Reduction
	* see the lecture for more information
	* TODO: Maybe write them here, later?
	
Допълнителни бележки:
* почти всеки алгоритъм има нужда от скалирани данни
	* няма да сбъркаме ако скалираме всеки път
		* освен ако нямаме някакви много сложни нелинейни връзки които ще се загубят

Resources:
* [Shear transformation](https://en.wikipedia.org/wiki/Shear_mapping)
* [Analysis of Variance (ANOVA)](https://en.wikipedia.org/wiki/Analysis_of_variance)