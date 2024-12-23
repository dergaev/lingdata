## Пояснения к слоям:
asr@tsk2005f_@iad2006m - слой с автоматическим распознаванием речи на всем видео  
alemm@... - слои с автоматической лемматизацией слов для каждого слоя (слов с words@...)  
amorph@... - слои с автоматической морфологической разметкой (по частям речи)  
aeng@... - слои с автоматическим переводом слоев text@... на английский язык  

## Проблемы автоматической разметки
Отмечу тут, что задание выполнено при помощи Whisper AI (asr), Mystem (alemm и amorph) и DeepL (aeng). Для всех из них использовал регулярные вырадения (чтобы редактировать представление данных, выбирать нужные столбцы и строки в таблицах при экспорте из Elan'а, использовать только определенные части строк (например, только леммы/части речи в результате работы Mystem)) и excel (чтобы сводить таблицы и сопоставлять преобразованные аннотации с их исходными таймкодами).

### ASR
При автоматическом распознавании речи нейронная сеть не справляется с:
1. именами собтвенными (вроде Копатыч, Вольчек и др.);
2. заимствованиями (джаст ин кейс);
3. перескающими репликами при одновременном говорении участников (напр. на 12:24-12:31 слвоо 'вариант' распознано как 'время';
4. нечетким произнесением (ударяю/удаляю);
5. редукцией и подбором грамматически верной формы (иногда окончания -ой/-ое/-ый/-ая путаются, но нейронная сеть их не поправляет в зависимости от констекта (это ее и не учили делать));
6. просто редукцией (иногда нечетко произнесенные финали слов вообще не учитываются 'создатели' > 'создать').

### ALEMM
Иногда встречаются ошибки, основанные на функциональной омонимии: например, парсит форму 'вкуснее' как компаратив от 'вкусный' (но контекстуально это - предикатив). Не распознает PROPN и считает, что 'Питер' - форма 'питер' (просто NOUN). тяжело с заимствованиями и словами вроде '3D'. В остальном очень-очень хорошо! (Но случаев функциональной омонимии много... как и в жизни).

### AMORPH
Трудности с функциональной омонимией (например, разметка 'можно' как ADV, а не как PRAEDIC; 'вкуснее' как ADJ а не как PRAEDIC). Забавно, что форму 'вкусный' наоборот посчитали NOUN (как будто оно субстантивировалось). 'Прям' > 'прямой' > ADJ (вместо PART). Есть странности с частицами и другими маленькими словами (почему-то союз 'а' размечен как PART). Основываясь на ALEMM, размечает все proper noun как noun... и много других случаев, которые вписываются в прчины:
1. функциональная омонимия;
2. разметка alemm (в свою очередь - опирается на ошибки asr)
3. маленькие слова (свойственные устной речи в таких количествах - междометия, частицы и др.)

### Итог
Ошибки допускаются на каждом следующем уровне разметки.  
ASR неправильно распознает некоторые слова. 
Лемматизация проходит с ошибками из-за функциональной омонимии > выбор pos опирается на ошибки лемматизации (и добавляет новые ошибки, связанные в том числе и сослоем alemm).
