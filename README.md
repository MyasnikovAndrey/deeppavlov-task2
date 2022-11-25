# Задание 2
Организовать доступ к модели при помощи REST API    

## Как мы подняли сервер и организовали к нему доступ
После поднятия REST сервера с моделью командой <code>python -m deeppavlov riseapi ner_ontonotes_bert_mult -p 5005</code> он будет запущен на 5005 порту хост-машины.

После инициализации модели к ней можно будет обратиться по URL http://127.0.0.1:5005, при условии, что мы находимся в одной сети с хост-машиной.  
Чтобы организовать доступ к модели из разных сетей был использован сервис Ngrok

>Ngrok - это сервис, который позволяет сделать локальный порт доступным из интернета 

Если прописать в консоли ngrok:  <code>ngrok.exe http 5005</code>, то ngrok сгенерирует ссылку, перейдя по которой, мы сможем получить доступ к 5005 порту хост-машины через интернет  

Наша сгенерированная ссылка, для доступа к серверу с моделью (<em>Возможно будет ругаться антивирус</em>):  
<code>https://3beb-109-202-60-123.eu.ngrok.io</code>  

## Инструкция
Получить результаты работы модели, используя свой текст и наш сервер с моделью, можно двумя способами:
> 1) Через cmd 
> 2) Используя графическую оболочку   

## Способ 1  
1) Открываем cmd  
2) Отправлять запрос POST мы будем с помощью инструмента curl  
>curl — инструмент для передачи данных с сервера или на него  

Шаблон запроса POST:  
<code>curl -X POST "https://3beb-109-202-60-123.eu.ngrok.io/model" -H "accept: application/json" -H "Content-Type: application/json" 
-d "{\"context_raw\":[\"Ваш_текст\"], \"question_raw\":[\"Вопрос_по_вашему_тексту\"]}"</code> 

Тестовый пример:
<code>curl -X POST "https://3beb-109-202-60-123.eu.ngrok.io/model" -H "accept: application/json" -H "Content-Type: application/json" -d "{\"context_raw\":[\"DeepPavlov is a library for NLP and dialog systems.\"], \"question_raw\":[\"What is DeepPavlov?\"]}"</code>
  
## Способ 2  
1) Перейти по ссылке <code>https://3beb-109-202-60-123.eu.ngrok.io</code>  
2) Вы увидите графическую оболочку, как на картинке ниже  
![image info](https://github.com/MyasnikovAndrey/deeppavlov-task2/blob/main/pictures/1.png)  
3)Нажимите **Try it out** в вверхнем правом углу  
![image info](https://github.com/MyasnikovAndrey/deeppavlov-task2/blob/main/pictures/2.png)  
  
На картинке выше представлены:
> request body - поле, куда мы пишим код  
> кнопка execute, которая посылает запрос  
> responses - типы ответов (200 - OK, 422 - Ошибка)  

Шаблон кода в request body должен выглядеть так:  
<code> {  
  "context_raw": [  
    "какой-то_ваш_текст"  
  ],  
  "question_raw": [  
    "вопрос_по_вашему_тексту"  
  ]  
}</code>   
  
Впишим код ниже в request body и нажмем кнопку execute:  
<code> {  
  "context_raw": [  
    "DeepPavlov is a library for NLP and dialog systems."  
  ],  
  "question_raw": [  
    "What is DeepPavlov?"  
  ]  
}</code>   

Получим следующий output:  
![image info](https://github.com/MyasnikovAndrey/deeppavlov-task2/blob/main/pictures/3.png)  
Ответ получился: <code>a library for NLP and dialog systems</code>  
Ответ сервера: 200 - все ОК
