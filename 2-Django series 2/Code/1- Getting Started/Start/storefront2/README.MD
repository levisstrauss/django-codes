--------------------API IN DJANGO----------------->
 
    Documentation: django-rest-framework.org
    ------------ Set up the project-------------
    -> Create a new database
    -> Configure the database settings
    -> pipenv install -> To install all the depenencies
    -> pipenv shell to activate the virtual environment
    -> python manage.py migrate -> To create all the databases
    -> python manage.py runserver -> To lunch the server
    -> python manage.py createsuperuser -> For the admin panel
    -> Username: admin, email: studentcontentcreator, pwd: 123456


     - API: Application Programming Interface
     - RESTFUL: Representational State Transfer\
     - Ressources are access uisng url
     - URL: Uniform Resource Locator
     - A resource can be represent in HTML, XML, JSON format
     - JSON: Javascript Object Notation

     - HTTP METHOS: Hypertext transfer protocol
        -> Get, Post, Put, Patch, Delete
        -> use put when we want to update all the resource otherwise use patch

------------------ Django rest framework -------------->

    -> pipenv install djangorestframework
    -> Add it to the list of installed app as 'rest_framework'

------------------ Creating API views in the view module-------------->   

    - Create a view function in the view module
    - Create a urls.py file to map the view function with
        urlpatterns = [
        path('products/', views.product_list)
        ]
        This simply mean that if your have a url products, you should go inside of
        views module and grab the product_list function
    - If the views we are creating is in the store app, we have to import it in the base project urls
  
 --------- Serializer to convert a model instance to a dictionary------>
 
    - Create a new files serializers.py
    - Different between the internal and external representation

     # An example of product class serializer

       class ProductSerializer(serializers.Serializer):
            id = serializers.IntegerField()
            title = serializers.CharField(max_length=255)
            unit_price = serializers.DecimalField(max_digits=6, decimal_places=2)

     NB: A serializer is the data you want to share if client hit the endpoint

     # An example of converting the product view function using a serializer

        # Create your views here.
        @api_view()
        def product_list(request):
            queryset = Product.objects.all()
            serializer = ProductSerializer(queryset, many=True)
            return Response(serializer.data)
        
        
        @api_view()
        def product_detail(request, id):
            product = get_object_or_404(Product, pk=id)
            serializer = ProductSerializer(product)
            return Response(serializer.data)

     - Sometimes we have the price showing as string, and to deal with that,
       Go in the setting panal at the end and include this:

        REST_FRAMEWORK = {
            'COERCE_DECIMAL_TO_STRING': False
        }

   ---------- Creating a Custom Serializer Fields ------>
   
     NB: in this exmple, the internal product class doesn't have a column 
         price_with_tax but we can make it by using the serializer and a function
         as shown in this code.


     class ProductSerializer(serializers.Serializer):
        id = serializers.IntegerField()
        title = serializers.CharField(max_length=255)
        unit_price = serializers.DecimalField(max_digits=6, decimal_places=2)
        price_with_tax = serializers.SerializerMethodField(method_name='calculate_tax')
    
        def calculate_tax(self, product):
            return product.unit_price * Decimal(1.1)

     - We can also change the name of all fields unit_price -> price
       price = serializers.DecimalField(max_digits=6, decimal_places=2, source='unit_price')

   ---------- Serializing relationships  ------> ....
   
----------------- Creating the Cart ---------------------------------->
      
     - Building an APi
         1- Serializer
         2- View
         3- Route
