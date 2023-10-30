# anova_tests

Проводим два дисперсионных анализа (*ANOVA*), один из которых однофакторный, второй - двухфакторный.<br>
Для первого в приложении по доставке готовых продуктов пользователям показывались фотографии блюд в разных разрешениях: прямоугольные 16:9, прямоугольные 12:4 и квадратные.
Для второго была обновлена кнопка заказа, и часть юзеров видела старый вариант, а часть – новый (две группы и два сегмента).

Файлы с данными загружены в репозиторий.

Первый содержит следующие поля:
- *id* – id клиента в эксперименте;
- *group* – в каком разрешении показывались картинки (A – прямоугольные 16:9, B – квадратные, C – прямоугольные 12:4);
- *events* – сколько блюд суммарно было заказано за период.

Второй файл:
- *id* – id клиента в эксперименте;
- *segment* – сегмент (high/low);
- *group* – вид кнопки (control – старая версия, test – новая версия);
- *events* – сколько блюд суммарно было заказано за период.

Используемые библиотеки:
- pandas;
- scipy;
- stats;
- seaborn;
- scipy.stats;
- numpy;
- statsmodels.
---

Тест 1

При проверке дисперсий внутри групп на гомогенность с помощью *теста Левена* увидели, что p-value = 0.1 (после округления до одного знака после запятой), то есть > 0.05, значит, **нулевую гипотезу** (что дисперсии гомогенны) **не отклоняем**.

После применения *теста Шапиро-Уилка* убедились, что **данные распределены нормально во всех трёх группах** (p-value > 0.05).

В результате использования *однофакторного дисперсионного анализа* увидели, что значение статистики равно 2886 (после округления до целого), а *p-value = 0.0.*

С помощью *критерия Тьюки* определили, что **статистически значимые различия встречаются между всеми группами** при сравнении их попарно. 
Так как наибольшее различие между группами B и A, **раскатываем вариант с квадратными картинками** на всех пользователей. 


Тест 2

Во втором эксперименте требуется проверить, как пользователи отреагируют на изменение формата кнопки оформления заказа, с разбивкой по сегменту клиента, поэтому будем использовать *многофакторный дисперсионный анализ*.

В рамках *statsmodels* используем формулу *events ~ segment + group + segment:group*, проводим тест через *anova_lm*.
После использования *критерия Тьюки* увидели, что **для обоих сегментов показатели статистически значимо увеличились по сравнению с контрольной группой**, разница между значением у тестовой группы сегмента low и контрольной группой этого же сегмента составила примерно 13, а между control/high и test/high - около 10.
