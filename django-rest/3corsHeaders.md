A Django App that adds Cross-Origin Resource Sharing (CORS) headers to responses. This allows in-browser requests to your Django application from other origins.

```bash
python -m pip install django-cors-headers

```
```py
INSTALLED_APPS = [
    ...,
    "corsheaders",
    ...,
]
```
```py
MIDDLEWARE = [
    ...,
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    ...,
]

```
`CorsMiddleware` should be placed as high as possible, especially before any middleware that can generate responses such as Django’s `CommonMiddleware` or `Whitenoise’s WhiteNoiseMiddleware`. If it is not before, it will not be able to add the CORS headers to these responses.

## configs

-    CORS_ALLOWED_ORIGINS
-    CORS_ALLOWED_ORIGIN_REGEXES
-    CORS_ALLOW_ALL_ORIGINS

```py
CORS_ALLOWED_ORIGINS = [
    "https://example.com",
    "https://sub.example.com",
    "http://localhost:8080",
    "http://127.0.0.1:9000",
]
```
