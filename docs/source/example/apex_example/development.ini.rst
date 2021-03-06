development.ini
===============

::

    [app:apex_example]
    use = egg:apex_example
    reload_templates = true
    debug_authorization = false
    debug_notfound = false
    debug_routematch = false
    debug_templates = true
    default_locale_name = en
    sqlalchemy.url = sqlite:///%(here)s/apex_example.db

    mako.directories = apex_example:templates

    apex.session_secret = apex_example_session_secret
    apex.auth_secret = apex_example_auth_secret
    apex.came_from_route = home

    [app:velruse]
    use = egg:velruse
    endpoint = http://domain.com/auth/apex_callback
    openid.store = openid.store.memstore:MemoryStore
    openid.realm = http://domain.com/

    providers =
        providers.twitter

    providers.twitter.consumer_key =
    providers.twitter.consumer_secret =

    [filter:exc]
    use=egg:WebError#evalerror

    [pipeline:papex_example]
    pipeline = exc tm apex_example

    [composite:main]
    use = egg:Paste#urlmap
    / = papex_example
    /velruse = velruse

    [filter:tm]
    use = egg:repoze.tm2#tm
    commit_veto = repoze.tm:default_commit_veto

    [server:main]
    use = egg:Paste#http
    host = 0.0.0.0
    port = 8080

    # Begin logging configuration

    [loggers]
    keys = root, apex_example, sqlalchemy

    [handlers]
    keys = console

    [formatters]
    keys = generic

    [logger_root]
    level = INFO
    handlers = console

    [logger_apex_example]
    level = DEBUG
    handlers =
    qualname = apex_example

    [logger_sqlalchemy]
    level = INFO
    handlers =
    qualname = sqlalchemy.engine
    # "level = INFO" logs SQL queries.
    # "level = DEBUG" logs SQL queries and results.
    # "level = WARN" logs neither.  (Recommended for production systems.)

    [handler_console]
    class = StreamHandler
    args = (sys.stderr,)
    level = NOTSET
    formatter = generic

    [formatter_generic]
    format = %(asctime)s %(levelname)-5.5s [%(name)s][%(threadName)s] %(message)s

    # End logging configuration
