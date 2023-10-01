# Детекция дефектов дорожного покрытия без размеченных данных с использование LiDAR

## Описание задачи
Протяженность дорог общего пользования в Москве превышает 6 тысяч километров и каждый год увеличивается не менее, чем на 100 километров. С постоянным ростом протяженности дорожной сети увеличиваются затраты и сложность своевременного обнаружения и устранения дефектов дорожного покрытия.
Такие дефекты являются причиной повышения аварийности на дорогах. Их профилактика (раннее обнаружение) позволяет значительно снизить затраты на ремонт покрытия и предотвратить аварийные ситуации.
Необходимо создать алгоритм, который будет детектировать дефекты с помощью неразмеченных данных с лидара.

## Основная концепция и этапы решения
Как можно за 48 часов, с помощью облака точек и без разметки создать модель, которая будет определять дефекты? Было принято решение использовать механизм кластеризации.
Этапы:
1. **Конвертация данных:** Преобразование данных из формата ROS2 в JSON.
2. **Предобработка данных:** Исправление аномалий и дефектов в данных.
3. **Объединение данных:** Слияние нескольких кадров для улучшения разрешения с использованием специализированных алгоритмов.
4. **Отбор данных:** Выделение точек, представляющих собой дорожное покрытие.
   ![Визуализация точек соответсвующих дороге](https://github.com/HeinrichWirth/health_of_road/blob/main/images/road.jpg)
5. **Зона детекции:** Определение области вокруг беспилотного транспортного средства с максимальной плотностью точек.
   ![Визуализация точек соответсвующих области поиска](https://github.com/HeinrichWirth/health_of_road/blob/main/images/detection_area.png)
6. **Кластеризация:** Использование DBSCAN для идентификации дефектов, опираясь на отклонения в 4 и более сигм.

## Используемые технологии
- **ROS2** для обработки исходных данных.
- **Open3D** для кластеризации и обработки облака точек.
- **Python** в качестве основного языка программирования.
- **DBSCAN** как метод кластеризации.

## Трудности
Данные оказались очень "сырыми".
А именно:
1. **Недостаточное разрешение:** Из-за низкой плотности точек лидара пришлось комбинировать данные из нескольких кадров, используя алгоритм ICP.
2. **Ошибки в координатах:** Наблюдались аномалии в координатах оси Z, что потребовало применения алгоритма RANSAC для коррекции.

   **Изначальные данные**.
- Цвет показывает координаты по оси Z:

   ![z-coord](https://github.com/HeinrichWirth/health_of_road/blob/main/images/z-coord.jpg)

- Вид на данные с числовой шкалой

   ![z-coord_3d](https://github.com/HeinrichWirth/health_of_road/blob/main/images/z-coord_3d.jpg)

- Исправленные данные
   ![fixed_z-coord](https://github.com/HeinrichWirth/health_of_road/blob/main/images/fixed_z-coord.jpg)

## Результаты
Был разработан эффективный алгоритм, который может детектировать дефекты дорожного покрытия, используя данные с лидара и GPS. Единственным недостатком осталось отсутствие веб-интерфейса для визуализации результатов на карте.
