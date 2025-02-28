# sanic-restful-resources
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmichaelkrukov%2Fsanic-restful-resources.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmichaelkrukov%2Fsanic-restful-resources?ref=badge_shield)


> Simple library for creating RESTful APIs with sanic.

## Features

- simplicity
- schematics integration
- exceptions handling
- unified response format
- 100% coverage

## Usage

```bash
python3 -m pip install sanic-restful-resources
```

## Example

```py
users = [{'name': 'Michael'}, {'name': 'Ivan'}]


class UserPostSchema(Model):
    name = StringType(required=True)


@resource('/users')
class Users:
    async def get(self, request):
        return users

    @validate(user_data=UserPostSchema)
    async def post(self, request, user_data):
        users.append({'name': user_data.name})
        return '', 204
```

## Guide

- `resource(uri='')` - return decorator for classes that will be
  considered RESTful resources. This decorator automatically extends
  HTTPMethodView (refer to sanic documentation for details). You can
  provide resource URI through decorator or by class attribute `uri`.
  You can provide decorators for all the methods through class
  attribute `decorators`.

  Examples of possible return values for handlers:

  ```py
  return "data", 200, {"X-Custom-Header": "Value"}
  return "data", 200
  return "data"
  return {"arg": "val"}
  return ["val1", "val2"]
  return "", 201

  return sanic.response.json # .text, .html, e.t.c.
  ```

- `Api(name='API', url_prefix)` - class for aggregating resources
  and registrating them in the sanic application. Internally it uses
  blueprints. Basic workflow:

  ```py
  from sanic import Sanic
  from resources import User, Users

  app = Sanic()

  api = Api(url_prefix='/api/v1')
  api.add_resource(User)
  api.add_resource(Users)
  api.init_app(app)

  # ...
  ```

- `validate(**models)` - decorator for methods, that will validate
  incoming data with provided models from Schematics library. Successfully
  validated and parsed models will be passed as keyword arguments to
  the handler method. If any model fails to validate - handler will
  not be called.

- `error(description=None, details=None, status=400, **kwargs` - method
  for returning errors from handlers.

- `collect_args(request)` - method for getting data from all possible
  sources of data in the request.


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fmichaelkrukov%2Fsanic-restful-resources.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fmichaelkrukov%2Fsanic-restful-resources?ref=badge_large)