title: flask.helpers url_for Python code examples
category: page
slug: flask-helpers-url-for-examples
sortorder: 500021002
toc: False
sidebartitle: flask.helpers url_for
meta: Python example code for the url_for function from the flask.helpers module of the Flask project.


url_for is a function within the flask.helpers module of the Flask project.


## Example 1 from Flask AppBuilder
[Flask-AppBuilder](https://github.com/dpgaspar/Flask-AppBuilder)
([documentation](https://flask-appbuilder.readthedocs.io/en/latest/)
and
[example apps](https://github.com/dpgaspar/Flask-AppBuilder/tree/master/examples))
is a web application generator that uses Flask to automatically create
the code for database-driven applications based on parameters set
by the user. The generated applications include default security settings,
forms, and internationalization support.

Flask App Builder is provided under the
[BSD 3-Clause "New" or "Revised" license](https://github.com/dpgaspar/Flask-AppBuilder/blob/master/LICENSE).

[**Flask AppBuilder / flask_appbuilder / menu.py**](https://github.com/dpgaspar/Flask-AppBuilder/blob/master/flask_appbuilder/./menu.py)

```python
# menu.py
from typing import List

~~from flask import current_app, url_for
from flask_babel import gettext as __

from .api import BaseApi, expose
from .basemanager import BaseManager
from .security.decorators import permission_name, protect


class MenuItem(object):
    def __init__(self, name, href="", icon="", label="", childs=None, baseview=None):
        self.name = name
        self.href = href
        self.icon = icon
        self.label = label
        self.childs = childs or []
        self.baseview = baseview

    def get_url(self):
        if not self.href:
            if not self.baseview:
                return ""
            else:
~~                return url_for(f"{self.baseview.endpoint}.{self.baseview.default_view}")
        else:
            try:
~~                return url_for(self.href)
            except Exception:
                return self.href

    def __repr__(self):
        return self.name


class Menu(object):
    def __init__(self, reverse=True, extra_classes=""):
        self.menu = []
        if reverse:
            extra_classes = extra_classes + "navbar-inverse"
        self.extra_classes = extra_classes

    @property
    def reverse(self):
        return "navbar-inverse" in self.extra_classes

    def get_list(self):
        return self.menu

    def get_flat_name_list(self, menu: "Menu" = None, result: List = None) -> List:
        menu = menu or self.menu
        result = result or []


## ... source file continues with no further url_for examples...


```


## Example 2 from FlaskBB
[FlaskBB](https://github.com/flaskbb/flaskbb)
([project website](https://flaskbb.org/)) is a [Flask](/flask.html)-based
forum web application. The web app allows users to chat in an open
message board or send private messages in plain text or
[Markdown](/markdown.html).

FlaskBB is provided as open source
[under this license](https://github.com/flaskbb/flaskbb/blob/master/LICENSE).

[**FlaskBB / flaskbb / markup.py**](https://github.com/flaskbb/flaskbb/blob/master/flaskbb/./markup.py)

```python
# markup.py
# -*- coding: utf-8 -*-
"""
    flaskbb.utils.markup
    --------------------

    A module for all markup related stuff.

    :copyright: (c) 2016 by the FlaskBB Team.
    :license: BSD, see LICENSE for more details.
"""
import logging
import re

import mistune
~~from flask import url_for
from jinja2 import Markup
from pluggy import HookimplMarker
from pygments import highlight
from pygments.formatters import HtmlFormatter
from pygments.lexers import get_lexer_by_name
from pygments.util import ClassNotFound

impl = HookimplMarker('flaskbb')

logger = logging.getLogger(__name__)

_re_user = re.compile(r'@(\w+)', re.I)


def userify(match):
    value = match.group(1)
    user = "<a href='{url}'>@{user}</a>".format(
~~        url=url_for("user.profile", username=value, _external=False),
        user=value
    )
    return user


class FlaskBBRenderer(mistune.Renderer):
    """Markdown with some syntactic sugar, such as @user gettting linked
    to the user's profile.
    """

    def __init__(self, **kwargs):
        super(FlaskBBRenderer, self).__init__(**kwargs)

    def paragraph(self, text):
        """Render paragraph tags, autolinking user handles."""

        text = _re_user.sub(userify, text)
        return super(FlaskBBRenderer, self).paragraph(text)

    def block_code(self, code, lang):
        if lang:
            try:
                lexer = get_lexer_by_name(lang, stripall=True)
            except ClassNotFound:


## ... source file continues with no further url_for examples...


```


## Example 3 from flask-base
[flask-base](https://github.com/hack4impact/flask-base)
([project documentation](http://hack4impact.github.io/flask-base/))
provides boilerplate code for new [Flask](/flask.html) web apps.
The purpose of the boilerplate is to stitch together disparate
libraries that are commonly used in Flask projects, such as
[Redis](/redis.html) for fast caching and transient data storage,
[SendGrid](https://www.twilio.com/sendgrid) for transactional email,
[SQLAlchemy](/sqlalchemy.html) for persistent data storage through a
[relational database](/databases.html) backend,
[Flask-WTF](https://flask-wtf.readthedocs.io/en/stable/) for form
handling and many others.

flask-base is provided as open source under the
[MIT license](https://github.com/hack4impact/flask-base/blob/master/LICENSE.md).

[**flask-base / app / utils.py**](https://github.com/hack4impact/flask-base/blob/master/app/./utils.py)

```python
# utils.py
~~from flask import url_for
from wtforms.fields import Field
from wtforms.widgets import HiddenInput
from wtforms.compat import text_type


def register_template_utils(app):
    """Register Jinja 2 helpers (called from __init__.py)."""

    @app.template_test()
    def equalto(value, other):
        return value == other

    @app.template_global()
    def is_hidden_field(field):
        from wtforms.fields import HiddenField
        return isinstance(field, HiddenField)

    app.add_template_global(index_for_role)


def index_for_role(role):
~~    return url_for(role.index)


class CustomSelectField(Field):
    widget = HiddenInput()

    def __init__(self, label='', validators=None, multiple=False,
                 choices=[], allow_custom=True, **kwargs):
        super(CustomSelectField, self).__init__(label, validators, **kwargs)
        self.multiple = multiple
        self.choices = choices
        self.allow_custom = allow_custom

    def _value(self):
        return text_type(self.data) if self.data is not None else ''

    def process_formdata(self, valuelist):
        if valuelist:
            self.data = valuelist[1]
            self.raw_data = [valuelist[1]]
        else:
            self.data = ''


## ... source file continues with no further url_for examples...


```


## Example 4 from flaskex
[Flaskex](https://github.com/anfederico/Flaskex) is a working example
[Flask](/flask.html) web application intended as a base to build your
own applications upon. The application comes with pre-built sign up, log in
and related screens, as well as a database backend. Flaskex is provided
as open source under the
[MIT license](https://github.com/anfederico/Flaskex/blob/master/LICENSE.txt).

[**flaskex / app.py**](https://github.com/anfederico/Flaskex/blob/master/././app.py)

```python
# app.py
# -*- coding: utf-8 -*-

from scripts import tabledef
from scripts import forms
from scripts import helpers
~~from flask import Flask, redirect, url_for, render_template, request, session
import json
import sys
import os

app = Flask(__name__)
app.secret_key = os.urandom(12)  # Generic key for dev purposes only

# Heroku
#from flask_heroku import Heroku
#heroku = Heroku(app)

# ======== Routing =========================================================== #
# -------- Login ------------------------------------------------------------- #
@app.route('/', methods=['GET', 'POST'])
def login():
    if not session.get('logged_in'):
        form = forms.LoginForm(request.form)
        if request.method == 'POST':
            username = request.form['username'].lower()
            password = request.form['password']
            if form.validate():
                if helpers.credentials_valid(username, password):
                    session['logged_in'] = True
                    session['username'] = username
                    return json.dumps({'status': 'Login successful'})
                return json.dumps({'status': 'Invalid user/pass'})
            return json.dumps({'status': 'Both fields required'})
        return render_template('login.html', form=form)
    user = helpers.get_user()
    return render_template('home.html', user=user)


@app.route("/logout")
def logout():
    session['logged_in'] = False
~~    return redirect(url_for('login'))


# -------- Signup ---------------------------------------------------------- #
@app.route('/signup', methods=['GET', 'POST'])
def signup():
    if not session.get('logged_in'):
        form = forms.LoginForm(request.form)
        if request.method == 'POST':
            username = request.form['username'].lower()
            password = helpers.hash_password(request.form['password'])
            email = request.form['email']
            if form.validate():
                if not helpers.username_taken(username):
                    helpers.add_user(username, password, email)
                    session['logged_in'] = True
                    session['username'] = username
                    return json.dumps({'status': 'Signup successful'})
                return json.dumps({'status': 'Username taken'})
            return json.dumps({'status': 'User/Pass required'})
        return render_template('login.html', form=form)
~~    return redirect(url_for('login'))


# -------- Settings ---------------------------------------------------------- #
@app.route('/settings', methods=['GET', 'POST'])
def settings():
    if session.get('logged_in'):
        if request.method == 'POST':
            password = request.form['password']
            if password != "":
                password = helpers.hash_password(password)
            email = request.form['email']
            helpers.change_user(password=password, email=email)
            return json.dumps({'status': 'Saved'})
        user = helpers.get_user()
        return render_template('settings.html', user=user)
~~    return redirect(url_for('login'))


# ======== Main ============================================================== #
if __name__ == "__main__":
    app.run(debug=True, use_reloader=True, host="0.0.0.0")


## ... source file continues with no further url_for examples...


```


## Example 5 from flask-login
[Flask-Login](https://github.com/maxcountryman/flask-login)
([project documentation](https://flask-login.readthedocs.io/en/latest/)
and [PyPI package](https://pypi.org/project/Flask-Login/))
is a [Flask](/flask.html) extension that provides user session
management, which handles common tasks such as logging in
and out of a [web application](/web-development.html) and
managing associated user session data. Flask-Login is
open sourced under the
[MIT license](https://github.com/maxcountryman/flask-login/blob/master/LICENSE).

[**flask-login / flask_login / utils.py**](https://github.com/maxcountryman/flask-login/blob/master/flask_login/./utils.py)

```python
# utils.py
# -*- coding: utf-8 -*-
'''
    flask_login.utils
    -----------------
    General utilities.
'''


import hmac
from hashlib import sha512
from functools import wraps
from werkzeug.local import LocalProxy
from werkzeug.security import safe_str_cmp
from werkzeug.urls import url_decode, url_encode

~~from flask import (_request_ctx_stack, current_app, request, session, url_for,
                   has_request_context)

from ._compat import text_type, urlparse, urlunparse
from .config import COOKIE_NAME, EXEMPT_METHODS
from .signals import user_logged_in, user_logged_out, user_login_confirmed


#: A proxy for the current user. If no user is logged in, this will be an
#: anonymous user
current_user = LocalProxy(lambda: _get_user())


def encode_cookie(payload, key=None):
    '''
    This will encode a ``unicode`` value into a cookie, and sign that cookie
    with the app's secret key.

    :param payload: The value to encode, as `unicode`.
    :type payload: unicode

    :param key: The key to use when creating the cookie digest. If not
                specified, the SECRET_KEY value from app config will be used.
    :type key: str
    '''


## ... source file abbreviated to get to url_for examples ...


    c_url = urlparse(current_url)

    if (not l_url.scheme or l_url.scheme == c_url.scheme) and \
            (not l_url.netloc or l_url.netloc == c_url.netloc):
        return urlunparse(('', '', c_url.path, c_url.params, c_url.query, ''))
    return current_url


def expand_login_view(login_view):
    '''
    Returns the url for the login view, expanding the view name to a url if
    needed.

    :param login_view: The name of the login view or a URL for the login view.
    :type login_view: str
    '''
    if login_view.startswith(('https://', 'http://', '/')):
        return login_view
    else:
        if request.view_args is None:
~~            return url_for(login_view)
        else:
~~            return url_for(login_view, **request.view_args)


def login_url(login_view, next_url=None, next_field='next'):
    '''
    Creates a URL for redirecting to a login page. If only `login_view` is
    provided, this will just return the URL for it. If `next_url` is provided,
    however, this will append a ``next=URL`` parameter to the query string
    so that the login view can redirect back to that URL. Flask-Login's default
    unauthorized handler uses this function when redirecting to your login url.
    To force the host name used, set `FORCE_HOST_FOR_REDIRECTS` to a host. This
    prevents from redirecting to external sites if request headers Host or
    X-Forwarded-For are present.

    :param login_view: The name of the login view. (Alternately, the actual
                       URL to the login view.)
    :type login_view: str
    :param next_url: The URL to give the login view for redirection.
    :type next_url: str
    :param next_field: What field to store the next URL in. (It defaults to
                       ``next``.)
    :type next_field: str
    '''
    base = expand_login_view(login_view)



## ... source file continues with no further url_for examples...


```


## Example 6 from flask-phone-input
[flask-phone-input](https://github.com/miguelgrinberg/flask-phone-input)
is an example application that ties together the
[intTellInput.js](https://github.com/jackocnr/intl-tel-input)
JavaScript plugin with the
[Flask-WTF](https://flask-wtf.readthedocs.io/en/stable/) form-handling
library. flask-phone-input is provided as open source under the
[MIT license](https://github.com/miguelgrinberg/flask-phone-input/blob/1a1c227c044474ce0fe133493d7f8b0fb8312409/LICENSE).

[**flask-phone-input / app.py**](https://github.com/miguelgrinberg/flask-phone-input/blob/master/././app.py)

```python
# app.py
~~from flask import Flask, render_template, session, redirect, url_for
from flask_bootstrap import Bootstrap
from flask_wtf import FlaskForm
import phonenumbers
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired, ValidationError

app = Flask(__name__)
app.config['SECRET_KEY'] = 'top-secret!'
Bootstrap(app)


class PhoneForm(FlaskForm):
    phone = StringField('Phone', validators=[DataRequired()])
    submit = SubmitField('Submit')

    def validate_phone(self, phone):
        try:
            p = phonenumbers.parse(phone.data)
            if not phonenumbers.is_valid_number(p):
                raise ValueError()
        except (phonenumbers.phonenumberutil.NumberParseException, ValueError):
            raise ValidationError('Invalid phone number')


@app.route('/', methods=['GET', 'POST'])
def index():
    form = PhoneForm()
    if form.validate_on_submit():
        session['phone'] = form.phone.data
~~        return redirect(url_for('show_phone'))
    return render_template('index.html', form=form)


@app.route('/showphone')
def show_phone():
    return render_template('show_phone.html', phone=session['phone'])


## ... source file continues with no further url_for examples...


```


## Example 7 from flask-restx
[Flask RESTX](https://github.com/python-restx/flask-restx) is an
extension that makes it easier to build
[RESTful APIs](/application-programming-interfaces.html) into
your applications. Flask RESTX aims for minimal configuration to
get basic APIs running for existing applications and it exposes
endpoint documentation using [Swagger](https://swagger.io/).

Flask RESTX is provided as open source under the
[BSD  3-Clause license](https://github.com/python-restx/flask-restx/blob/master/LICENSE).

[**flask-restx / flask_restx / apidoc.py**](https://github.com/python-restx/flask-restx/blob/master/flask_restx/./apidoc.py)

```python
# apidoc.py
# -*- coding: utf-8 -*-
from __future__ import unicode_literals

~~from flask import url_for, Blueprint, render_template


class Apidoc(Blueprint):
    """
    Allow to know if the blueprint has already been registered
    until https://github.com/mitsuhiko/flask/pull/1301 is merged
    """

    def __init__(self, *args, **kwargs):
        self.registered = False
        super(Apidoc, self).__init__(*args, **kwargs)

    def register(self, *args, **kwargs):
        super(Apidoc, self).register(*args, **kwargs)
        self.registered = True


apidoc = Apidoc(
    "restx_doc",
    __name__,
    template_folder="templates",
    static_folder="static",
    static_url_path="/swaggerui",
)


@apidoc.add_app_template_global
def swagger_static(filename):
~~    return url_for("restx_doc.static", filename=filename)


def ui_for(api):
    """Render a SwaggerUI for a given API"""
    return render_template("swagger-ui.html", title=api.title, specs_url=api.specs_url)


## ... source file continues with no further url_for examples...


```


## Example 8 from flaskSaaS
[flaskSaas](https://github.com/alectrocute/flaskSaaS) is a boilerplate
starter project to build a software-as-a-service (SaaS) web application
in [Flask](/flask.html), with [Stripe](/stripe.html) for billing. The
boilerplate relies on many common Flask extensions such as
[Flask-WTF](https://flask-wtf.readthedocs.io/en/latest/),
[Flask-Login](https://flask-login.readthedocs.io/en/latest/),
[Flask-Admin](https://flask-admin.readthedocs.io/en/latest/), and
many others. The project is provided as open source under the
[MIT license](https://github.com/alectrocute/flaskSaaS/blob/master/LICENSE).

[**flaskSaaS / app / views / user.py**](https://github.com/alectrocute/flaskSaaS/blob/master/app/views/user.py)

```python
# user.py
~~from flask import (Blueprint, render_template, redirect, request, url_for,
                   abort, flash)
from flask.ext.login import login_user, logout_user, login_required, current_user
from itsdangerous import URLSafeTimedSerializer
from app import app, models, db
from app.forms import user as user_forms
from app.toolbox import email
# Setup Stripe integration
import stripe
import json
from json import dumps

stripe_keys = {
	'secret_key': "sk_test_GvpPOs0XFxeP0fQiWMmk6HYe",
	'publishable_key': "pk_test_UU62FhsIB6457uPiUX6mJS5x"
}

stripe.api_key = stripe_keys['secret_key']

# Serializer for generating random tokens
ts = URLSafeTimedSerializer(app.config['SECRET_KEY'])

# Create a user blueprint
userbp = Blueprint('userbp', __name__, url_prefix='/user')



## ... source file abbreviated to get to url_for examples ...


def signup():
    form = user_forms.SignUp()
    if form.validate_on_submit():
        # Create a user who hasn't validated his email address
        user = models.User(
            first_name=form.first_name.data,
            last_name=form.last_name.data,
            phone=form.phone.data,
            email=form.email.data,
            confirmation=False,
            password=form.password.data,
        )
        # Insert the user in the database
        db.session.add(user)
        db.session.commit()
        # Subject of the confirmation email
        subject = 'Please confirm your email address.'
        # Generate a random token
        token = ts.dumps(user.email, salt='email-confirm-key')
        # Build a confirm link with token
~~        confirmUrl = url_for('userbp.confirm', token=token, _external=True)
        # Render an HTML template to send by email
        html = render_template('email/confirm.html',
                               confirm_url=confirmUrl)
        # Send the email to user
        email.send(user.email, subject, html)
        # Send back to the home page
        flash('Check your emails to confirm your email address.', 'positive')
~~        return redirect(url_for('index'))
    return render_template('user/signup.html', form=form, title='Sign up')


@userbp.route('/confirm/<token>', methods=['GET', 'POST'])
def confirm(token):
    try:
        email = ts.loads(token, salt='email-confirm-key', max_age=86400)
    # The token can either expire or be invalid
    except:
        abort(404)
    # Get the user from the database
    user = models.User.query.filter_by(email=email).first()
    # The user has confirmed his or her email address
    user.confirmation = True
    # Update the database with the user
    db.session.commit()
    # Send to the signin page
    flash(
        'Your email address has been confirmed, you can sign in.', 'positive')
~~    return redirect(url_for('userbp.signin'))


@userbp.route('/signin', methods=['GET', 'POST'])
def signin():
    form = user_forms.Login()
    if form.validate_on_submit():
        user = models.User.query.filter_by(email=form.email.data).first()
        # Check the user exists
        if user is not None:
            # Check the password is correct
            if user.check_password(form.password.data):
		#Check if email is confirmed
		if user.confirmation == True:
			login_user(user)			
                	# Send back to the home page
			flash('Succesfully signed in.', 'positive')
~~			return redirect(url_for('userbp.account'))
		else:
                    flash('Confirm your email address first.', 'negative')
~~                    return redirect(url_for('userbp.signin'))
            else:
                flash('The password you have entered is wrong.', 'negative')
~~                return redirect(url_for('userbp.signin'))
        else:
            flash('Unknown email address.', 'negative')
~~            return redirect(url_for('userbp.signin'))
    return render_template('user/signin.html', form=form, title='Sign in')


@userbp.route('/signout')
def signout():
    logout_user()
    flash('Succesfully signed out.', 'positive')
~~    return redirect(url_for('index'))


@userbp.route('/account')
@login_required
def account():
    return render_template('user/account.html', title='Account')


@userbp.route('/forgot', methods=['GET', 'POST'])
def forgot():
    form = user_forms.Forgot()
    if form.validate_on_submit():
        user = models.User.query.filter_by(email=form.email.data).first()
        # Check the user exists
        if user is not None:
            # Subject of the confirmation email
            subject = 'Reset your password.'
            # Generate a random token
            token = ts.dumps(user.email, salt='password-reset-key')
            # Build a reset link with token
~~            resetUrl = url_for('userbp.reset', token=token, _external=True)
            # Render an HTML template to send by email
            html = render_template('email/reset.html', reset_url=resetUrl)
            # Send the email to user
            email.send(user.email, subject, html)
            # Send back to the home page
            flash('Check your emails to reset your password.', 'positive')
~~            return redirect(url_for('index'))
        else:
            flash('Unknown email address.', 'negative')
~~            return redirect(url_for('userbp.forgot'))
    return render_template('user/forgot.html', form=form)


@userbp.route('/reset/<token>', methods=['GET', 'POST'])
def reset(token):
    try:
        email = ts.loads(token, salt='password-reset-key', max_age=86400)
    # The token can either expire or be invalid
    except:
        abort(404)
    form = user_forms.Reset()
    if form.validate_on_submit():
        user = models.User.query.filter_by(email=email).first()
        # Check the user exists
        if user is not None:
            user.password = form.password.data
            # Update the database with the user
            db.session.commit()
            # Send to the signin page
            flash('Your password has been reset, you can sign in.', 'positive')
~~            return redirect(url_for('userbp.signin'))
        else:
            flash('Unknown email address.', 'negative')
~~            return redirect(url_for('userbp.forgot'))
    return render_template('user/reset.html', form=form, token=token)

@app.route('/user/pay')
@login_required
def pay():
    user = models.User.query.filter_by(email=current_user.email).first()
    if user.paid == 0:
    	return render_template('user/buy.html', key=stripe_keys['publishable_key'], email=current_user.email)
    return "You already paid."

@app.route('/user/charge', methods=['POST'])
@login_required
def charge():
    # Amount in cents
    amount = 500
    customer = stripe.Customer.create(email=current_user.email, source=request.form['stripeToken'])
    charge = stripe.Charge.create(
        customer=customer.id,
        amount=amount,
        currency='usd',
        description='Service Plan'
    )
    user = models.User.query.filter_by(email=current_user.email).first()
    user.paid = 1


## ... source file continues with no further url_for examples...


```


## Example 9 from Flasky
[Flasky](https://github.com/miguelgrinberg/flasky) is a wonderful
example application by
[Miguel Grinberg](https://github.com/miguelgrinberg) that he builds
while teaching developers how to use [Flask](/flask.html) in
[his books and videos](https://courses.miguelgrinberg.com/). Flasky
is [open sourced under the MIT license](https://github.com/miguelgrinberg/flasky/blob/master/LICENSE).

[**Flasky / app / models.py**](https://github.com/miguelgrinberg/flasky/blob/master/./app/models.py)

```python
# models.py
from datetime import datetime
import hashlib
from werkzeug.security import generate_password_hash, check_password_hash
from itsdangerous import TimedJSONWebSignatureSerializer as Serializer
from markdown import markdown
import bleach
~~from flask import current_app, request, url_for
from flask_login import UserMixin, AnonymousUserMixin
from app.exceptions import ValidationError
from . import db, login_manager


class Permission:
    FOLLOW = 1
    COMMENT = 2
    WRITE = 4
    MODERATE = 8
    ADMIN = 16


class Role(db.Model):
    __tablename__ = 'roles'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64), unique=True)
    default = db.Column(db.Boolean, default=False, index=True)
    permissions = db.Column(db.Integer)
    users = db.relationship('User', backref='role', lazy='dynamic')

    def __init__(self, **kwargs):
        super(Role, self).__init__(**kwargs)
        if self.permissions is None:


## ... source file abbreviated to get to url_for examples ...



    def is_following(self, user):
        if user.id is None:
            return False
        return self.followed.filter_by(
            followed_id=user.id).first() is not None

    def is_followed_by(self, user):
        if user.id is None:
            return False
        return self.followers.filter_by(
            follower_id=user.id).first() is not None

    @property
    def followed_posts(self):
        return Post.query.join(Follow, Follow.followed_id == Post.author_id)\
            .filter(Follow.follower_id == self.id)

    def to_json(self):
        json_user = {
~~            'url': url_for('api.get_user', id=self.id),
            'username': self.username,
            'member_since': self.member_since,
            'last_seen': self.last_seen,
~~            'posts_url': url_for('api.get_user_posts', id=self.id),
~~            'followed_posts_url': url_for('api.get_user_followed_posts',
                                          id=self.id),
            'post_count': self.posts.count()
        }
        return json_user

    def generate_auth_token(self, expiration):
        s = Serializer(current_app.config['SECRET_KEY'],
                       expires_in=expiration)
        return s.dumps({'id': self.id}).decode('utf-8')

    @staticmethod
    def verify_auth_token(token):
        s = Serializer(current_app.config['SECRET_KEY'])
        try:
            data = s.loads(token)
        except:
            return None
        return User.query.get(data['id'])

    def __repr__(self):
        return '<User %r>' % self.username


class AnonymousUser(AnonymousUserMixin):


## ... source file abbreviated to get to url_for examples ...


class Post(db.Model):
    __tablename__ = 'posts'
    id = db.Column(db.Integer, primary_key=True)
    body = db.Column(db.Text)
    body_html = db.Column(db.Text)
    timestamp = db.Column(db.DateTime, index=True, default=datetime.utcnow)
    author_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    comments = db.relationship('Comment', backref='post', lazy='dynamic')

    @staticmethod
    def on_changed_body(target, value, oldvalue, initiator):
        allowed_tags = ['a', 'abbr', 'acronym', 'b', 'blockquote', 'code',
                        'em', 'i', 'li', 'ol', 'pre', 'strong', 'ul',
                        'h1', 'h2', 'h3', 'p']
        target.body_html = bleach.linkify(bleach.clean(
            markdown(value, output_format='html'),
            tags=allowed_tags, strip=True))

    def to_json(self):
        json_post = {
~~            'url': url_for('api.get_post', id=self.id),
            'body': self.body,
            'body_html': self.body_html,
            'timestamp': self.timestamp,
~~            'author_url': url_for('api.get_user', id=self.author_id),
~~            'comments_url': url_for('api.get_post_comments', id=self.id),
            'comment_count': self.comments.count()
        }
        return json_post

    @staticmethod
    def from_json(json_post):
        body = json_post.get('body')
        if body is None or body == '':
            raise ValidationError('post does not have a body')
        return Post(body=body)


db.event.listen(Post.body, 'set', Post.on_changed_body)


class Comment(db.Model):
    __tablename__ = 'comments'
    id = db.Column(db.Integer, primary_key=True)
    body = db.Column(db.Text)
    body_html = db.Column(db.Text)
    timestamp = db.Column(db.DateTime, index=True, default=datetime.utcnow)
    disabled = db.Column(db.Boolean)
    author_id = db.Column(db.Integer, db.ForeignKey('users.id'))
    post_id = db.Column(db.Integer, db.ForeignKey('posts.id'))

    @staticmethod
    def on_changed_body(target, value, oldvalue, initiator):
        allowed_tags = ['a', 'abbr', 'acronym', 'b', 'code', 'em', 'i',
                        'strong']
        target.body_html = bleach.linkify(bleach.clean(
            markdown(value, output_format='html'),
            tags=allowed_tags, strip=True))

    def to_json(self):
        json_comment = {
~~            'url': url_for('api.get_comment', id=self.id),
~~            'post_url': url_for('api.get_post', id=self.post_id),
            'body': self.body,
            'body_html': self.body_html,
            'timestamp': self.timestamp,
~~            'author_url': url_for('api.get_user', id=self.author_id),
        }
        return json_comment

    @staticmethod
    def from_json(json_comment):
        body = json_comment.get('body')
        if body is None or body == '':
            raise ValidationError('comment does not have a body')
        return Comment(body=body)


db.event.listen(Comment.body, 'set', Comment.on_changed_body)


## ... source file continues with no further url_for examples...


```


## Example 10 from tedivms-flask
[tedivm's flask starter app](https://github.com/tedivm/tedivms-flask) is a
base of [Flask](/flask.html) code and related projects such as
[Celery](/celery.html) which provides a template to start your own
Flask web app. The project comes baked with an admin panel,
[API authentication and authorization](/application-programming-interfaces.html),
[SQLAlchemy](/sqlalchemy.html) and many other common libraries that are
often used with Flask.

The project's code is provided as open source under the
[BSD 2-Clause "Simplified" license](https://github.com/tedivm/tedivms-flask/blob/master/LICENSE.txt).

[**tedivms-flask / app / views / misc_views.py**](https://github.com/tedivm/tedivms-flask/blob/master/app/views/misc_views.py)

```python
# misc_views.py
# Copyright 2017 Twin Tech Labs. All rights reserved

from flask import Blueprint, redirect, render_template, current_app, abort
~~from flask import request, url_for, flash, send_from_directory, jsonify, render_template_string
from flask_user import current_user, login_required, roles_accepted

from app import db
from app.models.user_models import UserProfileForm, User, UsersRoles, Role
from app.utils.forms import ConfirmationForm
import uuid, json, os
import datetime

# When using a Flask app factory we must use a blueprint to avoid needing 'app' for '@app.route'
main_blueprint = Blueprint('main', __name__, template_folder='templates')

# The User page is accessible to authenticated users (users that have logged in)
@main_blueprint.route('/')
def member_page():
    if not current_user.is_authenticated:
~~        return redirect(url_for('user.login'))
    return render_template('pages/member_base.html')

# The Admin page is accessible to users with the 'admin' role
@main_blueprint.route('/admin')
@roles_accepted('admin')
def admin_page():
~~    return redirect(url_for('main.user_admin_page'))

@main_blueprint.route('/users')
@roles_accepted('admin')
def user_admin_page():
    users = User.query.all()
    return render_template('pages/admin/users.html', users=users)

@main_blueprint.route('/create_user', methods=['GET', 'POST'])
@roles_accepted('admin')
def create_user_page():
    if current_app.config.get('USER_LDAP', False):
        abort(400)

    form = UserProfileForm()
    roles = Role.query.all()
    form.roles.choices = [(x.id,x.name) for x in roles]

    if form.validate():
        user = User.query.filter(User.email == request.form['email']).first()
        if not user:
            user = User(email=form.email.data,
                        first_name=form.first_name.data,
                        last_name=form.last_name.data,
                        password=current_app.user_manager.hash_password(form.password.data),
                        active=True,
                        email_confirmed_at=datetime.datetime.utcnow())
            db.session.add(user)
            db.session.commit()
            allowed_roles = form.roles.data
            for role in roles:
                if role.id not in allowed_roles:
                    if role in user.roles:
                        user.roles.remove(role)
                else:
                    if role not in user.roles:
                        user.roles.append(role)
            db.session.commit()
            flash('You successfully created the new user.', 'success')
~~            return redirect(url_for('main.user_admin_page'))
        flash('A user with that email address already exists', 'error')
    return render_template('pages/admin/create_user.html', form=form)


@main_blueprint.route('/users/<user_id>/delete', methods=['GET', 'POST'])
@roles_accepted('admin')
def delete_user_page(user_id):
    if current_app.config.get('USER_LDAP', False):
        abort(400)
    form = ConfirmationForm()
    user = User.query.filter(User.id == user_id).first()
    if not user:
        abort(404)
    if form.validate():
        db.session.query(UsersRoles).filter_by(user_id = user_id).delete()
        db.session.query(User).filter_by(id = user_id).delete()
        db.session.commit()
        flash('You successfully deleted your user!', 'success')
~~        return redirect(url_for('main.user_admin_page'))
    return render_template('pages/admin/delete_user.html', form=form)


@main_blueprint.route('/users/<user_id>/edit', methods=['GET', 'POST'])
@roles_accepted('admin')
def edit_user_page(user_id):
    if current_app.config.get('USER_LDAP', False):
        abort(400)

    user = User.query.filter(User.id == user_id).first()
    if not user:
        abort(404)

    form = UserProfileForm(obj=user)
    roles = Role.query.all()
    form.roles.choices = [(x.id,x.name) for x in roles]

    if form.validate():
        if 'password' in request.form and len(request.form['password']) >= 8:
            user.password = current_app.user_manager.hash_password(request.form['password'])
        user.email = form.email.data
        user.first_name = form.first_name.data
        user.last_name = form.last_name.data
        user.active = form.active.data

        allowed_roles = form.roles.data
        for role in roles:
            if role.id not in allowed_roles:
                if role in user.roles:
                    user.roles.remove(role)
            else:
                if role not in user.roles:
                    user.roles.append(role)

        db.session.commit()
        flash('You successfully edited the user.', 'success')
~~        return redirect(url_for('main.user_admin_page'))

    form.roles.data = [role.id for role in user.roles]
    return render_template('pages/admin/edit_user.html', form=form)

@main_blueprint.route('/pages/profile', methods=['GET', 'POST'])
@login_required
def user_profile_page():
    if current_app.config.get('USER_LDAP', False):
        abort(400)

    # Initialize form
    form = UserProfileForm(request.form, obj=current_user)

    # Process valid POST
    if request.method == 'POST' and form.validate():
        # Copy form fields to user_profile fields
        form.populate_obj(current_user)

        # Save user_profile
        db.session.commit()

        # Redirect to home page
~~        return redirect(url_for('main.user_profile_page'))

    # Process GET or invalid POST
    return render_template('pages/user_profile_page.html',
                           current_user=current_user,
                           form=form)


## ... source file continues with no further url_for examples...


```
