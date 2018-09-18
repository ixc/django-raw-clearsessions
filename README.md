django-raw-clearsessions
===

By default, Django's `cached_db` and `db` session backends loop through deleted sessions in order to trigger pre/post delete signal handlers. This can be *extremely* slow and consume *a lot* of CPU and memory on sites with a lot of sessions.

This package provides a `SessionStoreMixin` class and alternative `cached_db` and `db` backends that use the private `_raw_delete()` method, which is much faster and consumes much less CPU and memory, but does not trigger pre/post delete signal handlers.


Usage
---

Add to your settings:

    SESSION_ENGINE = 'django_raw_clearsessions.cached_db'

Or:

    SESSION_ENGINE = 'django_raw_clearsessions.db'

Or:

    # mybackend.py
    from some.package import some_backend
    from django_raw_clearsessions import SessionStoreMixin


    class SessionStore(SessionStoreMixin, some_backend.SessionStore):
        pass

    # settings.py
    SESSION_ENGINE = 'mybackend'

Execute the Django `clearsessions` management command:

    $ python manage.py clearsessions
