# Анализ результатов A/B - тестирования: рекомендательная система

## Описание: 
Команда внедрила в приложение доставки умную систему рекомендации товаров – предполагается, что такая система поможет пользователям эффективнее работать с приложением и лучше находить необходимые товары. Чтобы проверить эффективность системы рекомендаций, был проведен АБ-тест. В группе 1 (тестовой) оказались пользователи с новой системой рекомендаций, в группе 0 (контрольной) пользователи со старой версией приложения, где нет рекомендации товаров. <br/>


**Сделать аналитическое заключение с ответом на вопрос, стоит ли включать новую систему рекомендаций на всех пользователей.**<br/>

**Для анализа использовался следующий стек технологий**: <br/>

![Python](https://img.shields.io/badge/-Python-0b0038?style=for-the-badge&logo=python&logoColor=3c78a9)
![Pandas](https://img.shields.io/badge/pandas-0b0038?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/numpy-0b0038?style=for-the-badge&logo=numpy&logoColor=4c74cc)
![Scipy](https://img.shields.io/badge/-Scipy-0b0038?style=for-the-badge&logo=scipy&logoColor=white)
![Statsmodels](https://img.shields.io/badge/statsmodels-0b0038?style=for-the-badge&logo=statsmodel&logoColor=white)
![Seaborn](https://img.shields.io/badge/seaborn-0b0038?style=for-the-badge&logo=seaborn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/matplotlib-0b0038?style=for-the-badge&logo=matplotlib&logoColor=white)
![Requests](https://img.shields.io/badge/requests-0b0038?style=for-the-badge&logo=requests&logoColor=white)

## Выполненные задачи:
  
1. Автоматизирован процесс загрузки исходных датасетов с использованием API Яндекс.Диска.

2. Проведен  разведочный анализ данных (EDA-анализ) в ходе которого для каждого датафрейма в данных было выявлено количество уникальных значений, пропусков, дубликатов и т.д.

3. Выбраны целевые метрики, характеризующие эффективность рекомендательной системы (среднее количество заказов на пользователя, среднее число товаров в заказе, ARPU, доля отмененных заказов).

4. Проведен расчет размера выборок в группах (control/test) и построены гистограммы распределений данных для каждой метрики.

5. Сформулированы нулевая (H0) и алтернативная гипотезы (H1).

6. Использованы методы статистического анализа для проверки гомогенности дисперсий (тест Левена) и нормальности распределения данных в группах (тест Шапиро-Уилка, тест Д'Агостино-Пирсона, Q-Q plot для визуальной оценки).

7. Выбраны и применены методы статистического анализа для оценки значимости изменений в метриках (T-test Уэлча, T-test Стьюдента, критерий Хи-квадрат Пирсона, Bootstrap)

8. Реализована функция, которая позволяет сравнить полученное по результатам статистического теста значение pvalue с заданным уровнем значимости alpha = 0,05 и сделать вывод о возможности отклонения нулевой гипотезы.
    
9. Проведена оценка результатов А/B-теста и сделано аналитическое заключение.

## Полученный результат:
При использовании новой системы рекомендаций товаров не обнаружено существенное изменение доли отмененных заказов или среднего количества продуктов в заказах, однако новая система статистически значимо увеличивает средний доход бизнеса с пользователя и среднее количество заказов на пользователя. <br/>
**Следовательно, новая система рекомендаций товаров считается эффективной и рекомендуется включать ее для всех пользователей.**

<!-- ## Исходные данные: 

1. **ab_users_data** - таблица c информацией о заказах:

    `user_id` - позаказный идентификатор пользователя; <br/>
    `order_id` - уникальный идентификатор заказа (номер чека);<br/>
    `action` - действие пользователя: create_order или cancel_order;<br/>
    `time` - время совершения действия;<br/> 
    `date` - дата совершения действия;<br/> 
    `group` - принадлежность пользователя к группе при АБ-тесте.

2. **ab_orders** - таблица с информацией о составе заказов:

    `order_id` - уникальный идентификатор заказа (номер чека);<br/>
    `creation_time` - время создания заказа;<br/> 
    `product_ids` - состав каждого заказа (в виде списка id товаров).<br/> 

3. **ab_products** - таблица с информацией товарных позициях:

    `product_id` - уникальный идентификатор товара;<br/>
    `name` - название товара;<br/> 
    `price` - цена товара.<br/>  -->

<!-- 
- Информация о заказах `df_orders`
- Информация о клиентах `df_customers`
- Информация о товарах в составе заказа `df_order_items` -->
<!-- <style>
ul {
    list-style-type: none; /* Убираем маркеры у ненумерованных списков */
    padding: 0; /* Убираем отступы */
}
</style>

<ul>

<details> 
    <summary>Информация о заказах `df_orders` <u>(см. подробнее)</u></summary>
    <p>

`order_id` - уникальный идентификатор заказа (номер чека)  
`customer_id` - позаказный идентификатор пользователя  
`order_status` - статус заказа  
`order_purchase_timestamp` - время создания заказа  
`order_approved_at` - время подтверждения оплаты заказа  
`order_delivered_carrier_date` - время передачи заказа в логистическую службу  
`order_delivered_customer_date` - время доставки заказа  
`order_estimated_delivery_date` - обещанная дата доставки  
</p>
</details>
</ul>

<ul>

<details> 
    <summary>Информация о заказах `df_customers` (см. подробнее)</summary>
    <p>

`customer_id` - позаказный идентификатор пользователя  
`customer_unique_id` - уникальный идентификатор пользователя (аналог номера паспорта)  
`customer_zip_code_prefix` - почтовый индекс пользователя  
`customer_city` - город доставки пользователя  
`customer_state` - штат доставки пользователя
</p>
</details>
</ul>

<ul>

<details> 
    <summary>Информация о заказах `df_order_items` (см. подробнее)</summary>
    <p>

`order_id` - уникальный идентификатор заказа (номер чека)  
`order_item_id` - идентификатор товара внутри одного заказа  
`product_id` - ид товара (аналог штрихкода)  
`seller_id` - ид производителя товара  
`shipping_limit_date` - максимальная дата доставки продавцом для передачи заказа партнеру по логистике  
`price` - цена за единицу товара  
`freight_value` - вес товара

</p>
</details>
</ul> -->
