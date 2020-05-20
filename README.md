Для запуска проекта выполните: 
    python manage.py runserver

# Примеры запросов из вебинара
from p_library.models import Book, Author

from django.db import models 

from django.db.models import Count, Sum, F

Book.objects.all()   

Author.objects.all()

annotated_authors = authors.annotate(Count("book_author")) 

authors_list = annotated_authors.filter(books__gt=1)

authors = Author.objects.annotate(books=Count("book_author")).filter(books__gt=1)

books = Book.objects.filter(author__in=authors_list)

from django.db.models import ExpressionWrapper

books_price = books.annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field="")))

books_price = books.annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field=DecimalField()))).aggregate(Sum("all_price"))

books_price = Book.objects.filter(author__in=authors_list).annotate(all_price=(ExpressionWrapper(F("price")*F("copy_count"), output_field=DecimalField()))).aggregate(Sum("all_price"))

from p_library.models import Reader 

reader = Reader(name="Вася")

reader.save()

reader.books.add(2) 

reader.books                                                                                                                   
<django.db.models.fields.related_descriptors.create_forward_many_to_many_manager.<locals.ManyRelatedManager at 0x7efbf14b4400

reader.books.all()                                                 
<QuerySet [<Book: Руслан и Людмила]

new_br = BookReading(book_id=2, reader_id=1, completion=False)

new_br.save()                                       

new_br
<BookReading: Руслан и Людмила-Вася-False
