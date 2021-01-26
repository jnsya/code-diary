# Django

## Questions
- It seems like the naming gives an entire directory to a model. So rather than in rails where you'd put a user model in `app/models/user.rb`, in Django it would be `user/models.py`. Is that right?

- Also MVC-like
- But naming conventions differ: (generic term: Django name)
  - Model: model
  - View: template
  - Controller: view

- Gain access to functions by importing (rather than inheriting from some base class, as in Rails).

## Misc
- Routes file: `urls.py`
- `models.py` is akin to the database schema.
  - lists the models and their attributes.
- `settings.py` is like the config directory

- often uses a `slug` for a URL
  - a slug is a readable addition to the url, taken from the resource's content
  - eg: https://stackoverflow.com/questions/427102/what-is-a-slug-in-django
    - `what-is-a-slug-in-django` is the slug.

