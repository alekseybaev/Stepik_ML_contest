##Описание

Представленное мной решение соревнования является заключительным этапом курса ["Введение в Data Science и машинное обучение"](https://stepik.org/course/4852) на Stepik.

Задание заключается в том, чтобы предсказать сможет ли пользователь успешно закончить онлайн курс "Анализ данных в R" (Stepik). Мы будем считать, что пользователь успешно закончил курс, если он правильно решил больше 40 практических заданий.

В данных:

    submission_data_test.csv
    events_data_test.csv

хранится информация о решениях и действиях для 6184 студентов за первые два дня прохождения курса. Это 6184 студентов, которые проходили курс в период с мая 2018 по январь 2019.

Используя данные о первых двух днях активности на курсе нужно предсказать, наберет ли пользователь более 40 баллов на курсе или нет.

В этих данных, вам доступны только первые дня активности студентов для того, чтобы сделать предсказание. На самом деле, используя эти данные, вы уже можете сделать прогноз. Например, если пользователь за первые два дня набрал 40 баллов, скорее всего он наберет более 40 баллов в дальнейшем. Чтобы подкрепить такие гипотезы, вы можете использовать данные, на которые мы исследовали в первых двух модулях курса, где для всех пользователей представлены все данные об их активности на курсе.

##Оформление результатов

Итогом работы должен стать csv файл c предсказанием для каждого студента из тестовых данных. Пример предсказания выглядит [следующим образом](https://stepik.org/media/attachments/course/4852/submission_example.csv).

Чтобы узнать точность ваших предсказаний, в качестве решения этого шага отпраьте файл с предсказаниями для каждого студента в указанном выше формате.

Убедитесь, что вы сформировали файл с предсказаними для всех 6184 студентов, для каждого студента должна быть предсказана вероятность, что он наберет более 40 баллов за курс. У вас есть 20 попыток засабмитить решения, в зачет пойдет наилучший вариант.

Результатом проверки этого задания будет значение ROC AUC score.

##Описание данных

    events_train.csv - данные о действиях, которые совершают студенты со стэпами
        step_id - id стэпа
        user_id - анонимизированный id юзера
        timestamp - время наступления события в формате unix date
        action - событие, возможные значения:
            discovered - пользователь перешел на стэп
            viewed - просмотр шага,
            started_attempt - начало попытки решить шаг, ранее нужно было явно 
                              нажать на кнопку - начать решение, перед тем как
                              приступить к решению практического шага
            passed - удачное решение практического шага

    submissions_train.csv - данные о времени и статусах сабмитов к практическим 
                            заданиям
        step_id - id стэпа
        timestamp - время отправки решения в формате unix date
        submission_status - статус решения
        user_id - анонимизированный id юзера

##Структура проекта
    
    notebooks - каталог с блокнотами решений и создания признаков для обучения моделей
    data - папка, в которой все датасеты
    models - содержит файл, в котором параметры настроенной модели
    submissions - папка с прогнозом
    

###Папка notebooks содержит:

    RF_model.ipynb - решение с использованием алгоритма Random Forest, основанное на датасете из 62 признаков созданных "вручную" без использования автоматической генерации. ROC_AUC = 0.89
    activity_weekday_week_mouthday.ipynb - определение активность для дня недели, недели года, дня месяца
    activity_features.ipynb - генерация признаков, основанных на активности для дня недели, недели года, дня месяца    
    order_days_based_target.ipynb - генерация признаков количества и распределения дней в течении первых 2 суток на курсе    
    EVENTS_features.ipynb - генерация признаков для events_train.csv и events_data_test.csv    
    SUBMISSIONS_features.ipynb - генерация признаков для submissions_train.csv и submission_data_test.csv    
    step_id_for_events.ipynb - определение порядка степов на курсе для events_train.csv и events_data_test.csv
    step_id_for_submissions.ipynb - определение порядка степов на курсе для submissions_train.csv и submission_data_test.csv    
    time_of_day_features.ipynb - генерация признаков, основанных на времени дня, в которое были действия на курсе   
    data_diff_SUBMISSIONS_EVENTS.ipynb - генерация признаков для разности дат между events_train и submissions_train    
    target_features.ipynb - формирование таргета   
    ALL_features.ipynb - сводная таблица признаков
    
Целевая пеменная принимает значение True, если пользователь имеет более 40 событий correct, и False в противном случае. В качестве модели используется RandomForest библиотеки sklearn для Python.

###Папка data содержит:
     
     events_dates.csv - таблица с датами в которые была активность пользователя на курсе, определенная для events_train.csv и events_data_test.csv
     submissions_dates.csv - таблица с датами в которые была активность пользователя на курсе, определенная для submissions_train.csv и submission_data_test.csv
     koef_week.csv - таблица активностей пользователей для номера недели
     koef_weekday.csv - таблица активностей пользователей для номера дня недели
     koef_day.csv - таблица активностей пользователей для номера дня месяца
     week_weekday_day.zip - таблица признаков, расчитанных  на активности пользователей дня недели, недели, дня месяца
     events_all_two_days.zip - выборка данных для 2-х дней из events_train.csv и events_data_test.csv
     submissions_all_two_days.zip - выборка данных для 2-х дней из submissions_train.csv и submission_data_test.csv
     days_feature_events_all.csv - таблица содержащая количество дней и их распределение в  events_train.csv и events_data_test.csv
     days_feature_submissions_all.csv - таблица содержащая количество дней и их распределение в submissions_train.csv и submission_data_test.csv
     target_feature.csv - датасет целевой переменной
     days_feature_target.csv - общая таблица количества дней и их распределения с учетом значений целевой переменной
     ideal_steps_events_all.csv - нормальный порядок степов для events_train.csv и events_data_test.csv
     feature_steps_events.zip - содержит признак последовательности прохождения степов пользователями
     times_feature_events_all_NEW.csv - признаки распределения активности пользователей по времени суток, по часам
     feature_data_events_22.zip - общая таблица признаков для events_train.csv и events_data_test.csv
     ideal_steps_submissions_all.csv - нормальный порядок степов для submissions_train.csv и submission_data_test.csv
     feature_data_submissions_16.zip - общая таблица признаков для submissions_train.csv и submission_data_test.csv
     users_count.csv - таблица пользователей, имеющих 198 passed и 76 correct
     events_all.zip - объединенная таблица events_train.csv и events_data_test.csv
     ideal_steps_events_all.csv - нормальный порядок степов для events_train.csv и events_data_test.csv
     daysdiff_ratio.csv - таблица признаков для разности дат между events_train и submissions_train
     train_data_19234_62.zip - общая таблица признаков для обучения модели
     test_data_6184_62.zip - общая таблица признаков для формирования сабмита
     
###Папка models содержит:    
     
     best_model.pkl - параметры модели, настроенной GridSearchCV 
     
###Папка submissions содержит:     
     
     RF_100_34_7_feature_62.csv - итоговый файл с прогнозом. ROC_AUC = 0.89
    
