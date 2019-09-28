### Django基础命令

#### 1.创建一个Django 项目

```python
django-admin startproject 项目名
```



#### 2.创建一个应用

```python
python manage.py startapp 应用名
```



#### 3. 运行服务器

```python
python manage.py runserver

python manage.py runserver 0.0.0.0:8000  
# 如果指定ip为0.0.0.0和端口，同网段内的主机都能进行访问，部署的时候使用
```



#### 4.执行文件迁移

```python
python manage.py migrate
```



#### 5.为模型生成迁移文件

```python
python manage.py makemigrations
```



#### 6.