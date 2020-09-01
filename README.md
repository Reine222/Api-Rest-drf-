# Api-Rest-drf-
Django Rest Framework

https://books.agiliq.com/projects/django-api-polls-tutorial/en/latest/

# Comment ordonner les models exemple d'un projet Blog

      -Categorie
           - Article
             - Commentaire
# Etape 1

       pip install djangorestframework
       
 # Etape 2
 
       INSTALLED_APPS = (
      ...
      'rest_framework',
      )

 
 # Etape 3
 
      projet/serializers.py
      
      
      from .models import Categorie, Article, Commentaire


      class CategorieSerializer(serializers.ModelSerializer):
          class Meta:
              model = Categorie
              fields = '__all__'


      class ArticleSerializer(serializers.ModelSerializer):
          related_name = CategorieSerializer(many=True, required=False)

          class Meta:
              model = Article
              fields = '__all__'


      class CommentaireSerializer(serializers.ModelSerializer):
          related_name = ArticleSerializer(many=True, read_only=True, required=False)

          class Meta:
              model = Commentaire
              fields = '__all__'
 
 
 # Etape 4
 
      projet/apiviews.py
      
      
      from rest_framework import viewsets

      from .models import Categorie, Article, Commentaire
      from .serializers import CtegorieSerializer, ArticleSerializer, CommentaireSerializer


      class CategorieViewSet(viewsets.ModelViewSet):
          queryset = Categorie.objects.all()
          serializer_class = CategorieSerializer
          
      class ArticleViewSet(viewsets.ModelViewSet):
          queryset = Article.objects.all()
          serializer_class = ArticleSerializer
          
      class CommentaireViewSet(viewsets.ModelViewSet):
          queryset = Commentaire.objects.all()
          serializer_class = CommentaireSerializer
          
      

 
 
 # Etape 5
 
      urls.py
      
      
      
      from rest_framework.routers import DefaultRouter
      from .apiviews import *


      router = DefaultRouter()
      router.register('categorie', CategorieViewSet, base_name='categorie')
      router.register('article', ArticleViewSet, base_name='article')
      router.register('commentaire', CommentaireViewSet, base_name='commentaire')


      urlpatterns = [
          # ...
      ]

      urlpatterns += router.urls
      
 
 # Etape 6
 
 
      views.py
      
      Def categorie(request):
      return render(request, 'pages/categorie.html'
      
      Def article(request):
      return render(request, 'pages/article.html'
      
      Def commentaire(request):
      return render(request, 'pages/commentaire.html'
 
 
