**Что вы должны сделать в консоли Django?**


```python
from news.models import *
```

1. Создать двух пользователей (с помощью метода `User.objects.create_user`).

   ```python
   user1 = User.objects.create_user(username='user1', password='111')
   user2 = User.objects.create_user(username='user2', password='222')
   ```

2. Создать два объекта модели Author, связанные с пользователями.
   ```python
   author1 = Author.objects.create(authorUser=user1)
   author2 = Author.objects.create(authorUser=user2)
   ```
3. Добавить 4 категории в модель Category.
   ```python
   cat_sports = Category.objects.create(name='sports')
   cat_politics = Category.objects.create(name='politics')
   cat_education = Category.objects.create(name='education')
   cat_technology = Category.objects.create(name='technology')
   ```
4. Добавить 2 статьи и 1 новость.
   ```python
   post_ar1 = Post.objects.create(
    author=author1, categoryType='AR', title='Статья 1', text='Текст статьи 1')
   post_ar2 = Post.objects.create(
      author=author1, categoryType='AR', title='Статья 2', text='Текст статьи 2')
   post_nw = Post.objects.create(
      author=author2, categoryType='NW', title='Новость', text='Текст новости')
   ```
5. Присвоить им категории (как минимум в одной статье/новости должно быть не меньше 2 категорий).

```python
   post_ar1.postCategory.add(cat_education, cat_technology)
   post_ar2.postCategory.add(cat_politics)
   post_nw.postCategory.add(cat_sports)
```

# =================

# Выходил из shell

6. Создать как минимум 4 комментария к разным объектам модели Post (в каждом объекте должен быть как минимум один комментарий).

```python
   post_ar1 = Post.objects.get(title='Статья 1')
   post_ar2 = Post.objects.get(title='Статья 2')
   post_nw = Post.objects.get(title='Новость')
   user1 = User.objects.get(username='user1')
   user2 = User.objects.get(username='user2')

   comment_ar1_us1 = Comment.objects.create(
      commentPost=post_ar1, commentUser=user1, text='Коммент user1 к статье 1')
   comment_ar2_us1 = Comment.objects.create(
      commentPost=post_ar2, commentUser=user1, text='Коммент user1 к статье 2')
   comment_ar1_us2 = Comment.objects.create(
      commentPost=post_ar2, commentUser=user2, text='Коммент user2 к статье 1')
   comment_ar2_us2 = Comment.objects.create(
      commentPost=post_ar2, commentUser=user2, text='Коммент user2 к статье 2')
   comment_nw_us1 = Comment.objects.create(
      commentPost=post_nw, commentUser=user1, text='Коммент user1 к новости')
   comment_nw_us2 = Comment.objects.create(
      commentPost=post_nw, commentUser=user2, text='Коммент user2 к новости')
```

7. Применяя функции like() и dislike() к статьям/новостям и комментариям, скорректировать рейтинги этих объектов.
   ```python
   post_ar1.like() # 5
   post_ar1.dislike() # 0
   post_ar2.like() # 3
   post_ar2.dislike() # 1
   post_nw.like() # 6
   post_nw.dislike() # 2
   comment_ar1_us1.like() # 4
   comment_ar1_us1.dislike() # 7
   comment_ar2_us1.like() # 5
   comment_ar2_us1.dislike() # 2
   comment_ar1_us2.like() # 4
   comment_ar1_us2.dislike() # 0
   comment_ar2_us2.like() # 6
   comment_ar2_us2.dislike() # 2
   comment_nw_us1.like() # 3
   comment_nw_us1.dislike() # 1
   comment_nw_us2.like() # 3
   comment_nw_us2.dislike() # 0
   ```
8. Обновить рейтинги пользователей.
   ```python
   Author.objects.get(pk=1).update_rating()
   Author.objects.get(pk=2).update_rating()
   ```
9. Вывести username и рейтинг лучшего пользователя (применяя сортировку и возвращая поля первого объекта).
   ```python
   best_author = Author.objects.order_by('-ratingAuthor').first()
   print(f'Имя пользователя: {best_author.authorUser.username}, Рейтинг: {best_author.ratingAuthor}')
   ```
10. Вывести дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи, основываясь на лайках/дислайках к этой статье.
    ```python
    best_post = Post.objects.filter(categoryType='AR').order_by('-rating').first()
    print(f'Дата: {best_post.dataCreation}, Автор: {best_post.author.authorUser.username}, Рейтинг: {best_post.rating}, Заголовок: {best_post.title}, Превью: {best_post.preview()}')
    ```
11. Вывести все комментарии (дата, пользователь, рейтинг, текст) к этой статье.

    ```python
    comments = Comment.objects.filter(commentPost=best_post)
    for comment in comments:
      print(f'Дата: {comment.dataCreation}, Имя пользователя: {comment.commentUser.username}, Рейтинг: {comment.rating}, Комментарий: {comment.text}')

    ```
