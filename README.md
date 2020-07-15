Для запуска проекта выполните: 
    python manage.py runserver

# Примеры запросов из вебинара
from p_library.models import Book, Author - импорт моделей необходимых для составления запросов

from django.db import models - модуля моделей

from django.db.models import Count, Sum, F - импорт спецификаторов запросов

Book.objects.all() - выборка всех записей из таблицы книга

Author.objects.all() - выборка всех записей из таблицы Автор

annotated_authors = authors.annotate(Count("book_author")) - аннотация количества авторов по признаку наличия произведений

authors_list = annotated_authors.filter(books__gt=1) - фильтрация списка авторов по признаку "более одного произведения в библиотеке" 

authors = Author.objects.annotate(books=Count("book_author")).filter(books__gt=1) - объединение двух предыдущих запросов

books = Book.objects.filter(author__in=authors_list) - фильтрация записей таблицы Книга по признаку нахождения автора в списке "authors"

from django.db.models import ExpressionWrapper - импорт обертки для написания составных запросов

books_price = books.annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field=""))) - аннотирование списка Книг общей стоимостью всех копий данной Книги

books_price = books.annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field=DecimalField()))).aggregate(Sum("all_price")) - аггрегации общей стоимости всех копий всех Книг

books_price = Book.objects.filter(author__in=authors_list).annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field=DecimalField()))).aggregate(Sum("all_price")) - объединение всех предыдущих запросов

from p_library.models import Reader - импорт модели Читатель

reader = Reader(name="Вася") - создание объекта нового Читателя

reader.save() - вызов метода сохранения объекта Читателя в БД

Использование RelatedManager:

reader.books.add(2) - добавление связи Читатель-Книга

reader.books - менеджер отношений RelatedManager для связи Читатель-Книга
<django.db.models.fields.related_descriptors.create_forward_many_to_many_manager.<locals.ManyRelatedManager at 0x7efbf14b4400 - 

reader.books.all() - запрос списка Книг, связанных с Читателем
<QuerySet [<Book: Руслан и Людмила]

Добавление связи Читатель-Книга с помощью создания записи в таблице связи:

new_br = BookReading(book_id=2, reader_id=1, completion=False)

new_br.save()                                       

new_br 
<BookReading: Руслан и Людмила-Вася-False
