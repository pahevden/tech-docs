
# 1. Введение

## 1.1 Назначение системы

Система автоматизации протоколов соревнований предназначена для упрощения работы судей и организаторов, а также для передачи данных режиссеру трансляции. Основные задачи:

- Автоматизированное создание протоколов соревнований на основе расписания.
- Экспорт стартовых и итоговых протоколов в формате PDF.
- Передача данных в реальном времени для отображения в трансляции.
- Упрощение ведения и корректировки информации по ходу соревнований.

## 1.2 Конечные пользователи

Система используется следующими группами пользователей:

- **Судьи** – ведут протоколы соревнований, вносят результаты.
- **Организаторы** – формируют расписание, управляют шаблонами протоколов.
- **Режиссер и его помощники** – получают данные из таблицы для использования в трансляции.

## 1.3 Основные функции

- Создание протоколов соревнований на основе расписания.
- Экспорт протоколов в PDF для печати и официальной документации.
- Обновление информации в режиме реального времени.
- Формирование данных для трансляции.

## 1.4 Связь с системой трансляции

Таблица содержит данные, необходимые для работы режиссера трансляции. Это может включать:

- Списки участников и их результаты.
- Расписание предстоящих заездов.
- Финальные итоги соревнований.
- Дополнительные данные, необходимые для комментаторов.

# 2. Архитектура системы

## 2.1 Структура таблицы

Google Таблица состоит из нескольких листов, каждый из которых выполняет свою функцию:

- **"Расписание"** – основной лист, содержащий данные о предстоящих заездах, их дисциплинах, категориях и временных параметрах.
- **"Список"** – содержит информацию об участниках соревнований.
- **"Справочники"** – статичные данные, используемые в расчетах и формировании протоколов.
- **Шаблоны протоколов** – различные листы, служащие основой для создания индивидуальных протоколов заездов.
- **Листы трансляции** – данные, передаваемые режиссеру для отображения информации в реальном времени.

## 2.2 Логика взаимодействия

1. **Организаторы** заполняют лист "Расписание" с данными о соревнованиях.
2. **Скрипт** обрабатывает этот лист и создает индивидуальные протоколы на основе шаблонов.
3. **Судьи** заполняют протоколы, фиксируя результаты заездов.
4. **Система экспортирует** стартовые и итоговые протоколы в PDF.
5. **Данные передаются** режиссеру трансляции для вывода в эфир.

## 2.3 Типы данных и ключевые параметры

- **Дисциплина** – вид соревнований (гит, спринт, скретч и др.).
- **Категория** – возрастная или спортивная категория участников.
- **Этап** – стадия соревнований (отборочный, полуфинал, финал и т. д.).
- **Время старта/финиша** – расчетное и фактическое время заезда.
- **Шаблон протокола** – определяет, на основе какого листа создается протокол.
- **PDF-ссылка** – формируется после экспорта протокола.

# 3. Основные функции скрипта

## 3.1 `createProtocols()` – создание протоколов

**Назначение:**  
Функция автоматически создает протоколы соревнований на основе данных из листа "Расписание".

**Алгоритм работы:**  
1. Проверяет, что активный лист – "Расписание".
2. Определяет количество строк для обработки из ячейки `A102`.
3. Проходит по строкам, считывает данные о дисциплинах, категориях, этапе.
4. Пропускает строки, если дисциплина отмечена как "Перерыв" или "Церемония награждения".
5. Создает новый лист на основе шаблона, указанного в "Расписании".
6. Заполняет ключевые параметры гонки.
7. Скрывает технические столбцы для удобства отображения и защиты формул.
8. Отмечает созданный протокол в "Расписании".

## 3.2 Запуск основных функций через меню

Основные функции по созданию протоколов и экспорту в PDF доступны через меню, создаваемое функцией `onOpen()`. После открытия таблицы в интерфейсе Google Sheets появляется меню "Ⓜ️ Меню Ⓜ️", включающее:

- **📝 Создать протоколы** – запускает `createProtocols()`.
- **🚴‍♂️ Экспорт Старт** – выполняет `exportSTARTProtocolToPDf()`.
- **🏁 Экспорт Финал** – выполняет `exportFINALProtocolToPDf()`.

# 4. Обработчики событий и вспомогательные функции

## 4.1 `onEdit(e)` – обработчик изменений в таблице

**Назначение:**  
Функция автоматически фиксирует изменения в таблице, например, время старта участников.

**Алгоритм работы:**  
1. Определяет, в каком столбце произошло изменение.
2. Если это столбец старта, фиксирует текущее время.
3. Обновляет статус участника в протоколе.

# 5. Возможные ошибки и способы их устранения

- Ошибка экспорта PDF – проверить диапазон данных.
- Не создается протокол – убедиться, что заполнены обязательные поля в "Расписании".
- Данные не передаются в трансляцию – проверить корректность листов трансляции.

Документация завершена.
