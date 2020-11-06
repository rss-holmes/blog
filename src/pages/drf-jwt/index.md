---
title: 'DRF and JWT'
date: '2020-10-15'
spoiler: The other kind of technical debt.
---

# DRF JWT Authentication

- All the functions in a drf pipe generally need to keep request and response objects which are part of the rest_framework
- All the functionality starts from the authentication class which can be overridden. The default authentication classes list mentioned in the settings file contains a list of classed each implementing an authenticate method and inheriting the `authentication.BaseAuthentication` from the rest_framework file.
- The authenticate function returns 2 objects , the first is the user object and the second the context object which can contain any context variables which need to be passed on to the next function in the pipe. The user is set into the `request.user` and the context object is set into the `request.auth`
- All the authentication functions included in the authentication classes list will be called one by one ideally. If any of the functions returns None then the next function in the list is automatically called. However if any function returns a non Null object then it breaks the chain and calls the next method in the chain
- In case any error is raised in this whole process , it is sent to the default error handler mentioned in the settings file
- In the next step the wsgi handler decides where to route the request. to do this the wsgi thread looks at the root url conf and then redirects it to the necessary view.
- Once inside the view we can implement all the business logic .Generally the view contains a mechanism to handle GET or POST data and then extract the key arguments and then pass the data through a serializer's validate method to ensure that the data conforms to a pre defined standard. Once the data is validated, other operations can be run on the data and then once the processing is complete the new data can be sent forward to the next step.
- In case any error occurs in this step the error is handled by the exception handler
- In case any exception is raised in the process enumerated till now , immediately the pipe breaks and the custom exception handler is called with an exception object. In case the exception is a subclass of `APIException`, it can be passed to the default exception_handler provided by the rest_framework to extract a meaningful response which if required can be sent directly to the api consumer. However in case the exception is not known to the rest framework meaning is not sub classing APIException then if we pass it to the default exception handler provided by the rest framework , it will return None as it wont be able to recognise it. In this case if not handled a 500 internal server error will be raised by wsgi. Therefore it is better we handle this scenario in a different function. Also any behaviour handled by these 2 functions can be overridden in small function blocks here only by maintaining an error class to handler class mapping and then calling the respective handler class depending on the error class.
- In the next step the data is sent to the default renderer .this class is responsible for reorganising all the data accumulated till now to make a meaningful response for the api consumer.

```bash
pip install djangorestframework-jwt
```

```python{4,19}
# the declaration of the drf settings in the settings file
# settings.py

REST_FRAMEWORK = {
	'DEFAULT_AUTHENTICATION_CLASSES': [
	'tranzact.rest_framework_customization.authentication.JWTAuthMiddlewareHTTP',
	'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
	'rest_framework.authentication.SessionAuthentication',
	'rest_framework.authentication.BasicAuthentication',
],

'DEFAULT_PERMISSION_CLASSES': [
	'rest_framework.permissions.IsAuthenticated',
],

'DEFAULT_RENDERER_CLASSES': (
	'tranzact.rest_framework_customization.renderer.CustomJSONRenderer',
),

'EXCEPTION_HANDLER': 'tranzact.rest_framework_customization.exception_handler.core_exception_handler',
}
```

```python{4,19}
# the custom authentication class
# authentication.py

class JWTAuthMiddlewareHTTP(authentication.BaseAuthentication):

    authentication_header_prefix = "Bearer"

    def authenticate(self, request):

        request.user = None

        # `auth_header` should be an array with two elements: 1) the name of
        # the authentication header (in this case, "Token") and 2) the JWT
        # that we should authenticate against.
        auth_header = authentication.get_authorization_header(request).split()
        auth_header_prefix = self.authentication_header_prefix.lower()

        if not auth_header:
            # No authentication header was provided so bypass the authentication mechanism
            return None

        if len(auth_header) != 2:
            # Authentication header wasnt supplied in the correct format
            msg = "Authentication header not provided in the correct format."
            raise exceptions.AuthenticationFailed(msg)

        # The JWT library we're using can't handle the `byte` type, which is
        # commonly used by standard libraries in Python 3. To get around this,
        # we simply have to decode `prefix` and `token`.
        prefix = auth_header[0].decode("utf-8")
        token = auth_header[1].decode("utf-8")

        if prefix.lower() != auth_header_prefix:
            # Authentication header prefix was not as specified
            msg = "Incorrect authentication header prefix supplied"
            raise exceptions.AuthenticationFailed(msg)

        return self._authenticate_credentials(request, token)

    def _authenticate_credentials(self, request, token):
        """
        Try to authenticate the given credentials. If authentication is
        successful, return the user and token. If not, throw an error.
        """
        try:
            user_decoded_jwt_data = jwt.decode(
                token, settings.SECRET_KEY, algorithms=["HS256"]
            )
        except:
            msg = "The authentication token could not be decoded."
            raise exceptions.AuthenticationFailed(msg)

        try:
            user = get_user_model().objects.select_related("userprofile").get(pk=user_decoded_jwt_data["user_id"])
        except get_user_model().DoesNotExist:
            msg = "No user matching this token was found."
            raise exceptions.AuthenticationFailed(msg)

        if not user.is_active:
            msg = "The user matching this token has been deactivated."
            raise exceptions.AuthenticationFailed(msg)
        
        # can return 2 objects, first will be set to request.user and 2nd to request.auth
        return (user, token)

```

```python{4,19}
# the view function which contains the business logic
# views.py

class LoginAPIView(APIView):
	permission_classes = (AllowAny,)
	#renderer_classes = (UserJSONRenderer,)
	serializer_class = LoginSerializer

	def post(self, request):
	
		user = request.data.get("user", {}
		serializer = self.serializer_class(data=user)
		serializer.is_valid(raise_exception=True)
		return Response(serializer.data, status=status.HTTP_200_OK)
```

```python{4,19}
# the custom error handler function
# exception_handler.py

from rest_framework.views import exception_handler, Response

def core_exception_handler(exc, context):

    response = exception_handler(exc, context)
    request = context["request"]
    exception_class = exc.__class__.__name__

    if response is None:

        # if the response is None it means that drf could not recognize the type of error.
        # This means the error is not a subclass of APIException but a subclass of Exception
        # and thus needs to be handled separately
        return _core_higher_order_exception_handler(exc, context)

    custom_lower_order_exception_handlers = {
        "ValidationError": _handle_validation_error,
        "PermissionDenied": _handle_permission_denied_error,
    }

    if exception_class in custom_lower_order_exception_handlers:
        return custom_lower_order_exception_handlers[exception_class](
            exc, context, response
        )

    send_error_email_manually(exc, context)

    return _handle_default_error(exc, context, response)

def _core_higher_order_exception_handler(exc, context):

    request = context["request"]
    response = Response(
        {"errors": {"exception": str(exc)}}, status=500, content_type="application/json"
    )
    exception_class = exc.__class__.__name__

    custom_higher_order_exception_handlers = {
        "ValueError": _handle_value_error,
    }

    if exception_class in custom_higher_order_exception_handlers:
        return custom_higher_order_exception_handlers[exception_class](
            exc, context, response
        )

    return response

def _handle_value_error(exc, context, response):

    return response

def _handle_validation_error(exc, context, response):

    response.data = {"errors": response.data}

    return response

def _handle_permission_denied_error(exc, context, response):

    response.data = {"errors": response.data}

    return response

def _handle_default_error(exc, context, response):

    response.data = {"errors": response.data}

    return response

```

```python{4,19}
# the view function which contains the rendering logic
# renderer.py

from rest_framework.renderers import JSONRenderer

class CustomJSONRenderer(JSONRenderer):
    charset = "utf-8"

    def render(self, data, accepted_media_type=None, renderer_context=None):

        # here data is whatever comes under the response.data section
        try:
        
            errors = data.get("errors", "")
            request = renderer_context["request"] or None
            anonymous = int(request.user.is_anonymous),

        except AttributeError:
            errors = ""
            request = renderer_context["request"] or None
            anonymous = int(request.user.is_anonymous),

        except: 
            errors = ""
            anonymous = 1

        data = {
            "data": data,
            "status": 0 if errors else 1,
            "message": errors,
            "anonymous": anonymous, 
        }

        return super(CustomJSONRenderer, self).render(
            data, accepted_media_type, renderer_context
        )
```

```python{4,19}
# In urls.py
from rest_framework_jwt.views import obtain_jwt_token, refresh_jwt_token

urlpatterns = [
url(r'^api-token-auth/', obtain_jwt_token),
url(r'^api-token-refresh/', refresh_jwt_token),
]
```