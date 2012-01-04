===============
User Management
===============

Features
--------

* one login to rule them all = Open Id Connect
* admin can manage users
* user can register himself
* user can edit profile data consisting of name, bio, email, homepage
* user can ask for forgotten password
* double opt-in

Later:
* session will store came_from for double opt-in flow
* Login per Facebook, Twitter, Google+
* add more to profile data: links, picture, ...


Use Cases
---------

User registers an account
*************************

1. User clicks the "Register" button
2. System shows registration form
3. User fills out the form and sends it
4. User creates new user and send opt-in email, logs user in
5. User clicks on link in email
6. system activates user and logs user in


- for now: you can only login with activation
- user will get message to activate if he hasn't yet
- user can resend activation code by entereing the email address
- activation also means login
- activation code is only valid for x days
- after that it needs to be resend


User logs in  
************

1. User enters email address and password
2. System logs user in and send to callback address (as target=_top)

- if password is wrong let user try again
- after 10 tries enhance waiting time by 1 minute, then by last waiting time (blocked_until)
- if user is not activated, tell him and give option to resend the activation code
- if email or password is wrong only say "email or password wrong" 
- give link to reset password


User resets password
********************

1. User enters email address, maybe taken over from login form
2. System sends a link for setting a new password
3. User enters new password twice and is logged in

- Similar to activation link: only valid for x days
- If code is wrong simply say so. Maybe give link to resend reset code


Admin
*****

Administrators should be able to do the following:

- Add users manually with auto generated passwords send to the user
- Edit users
- set activation and password reminder timeouts (pref screen)
- set default callback if none was given
- Define all the mail templates
- Delete users or block users
- Manually send them the password reset
- Manually activate users
- see last logged in dates


Technical details
-----------------

User Record:

* user_id (auto generated)
* email address (also used for login)
* password md5 hash
* last logged in date
* created
* last edited
* nickname (optional)
* full name (optional)
* biography (optional)
* homepage (optional)


Database classes
----------------

- Classes derive from ``Document`` class
- Attributes can be access by simple dot or array notation
- fields are defined in FIELDS dictionary

Example::

    class User(Document):

	_fields = dict(
	    user_id =	IdField(),
	    email =	EMailField(required=True),
	    nickname =	StringField(maxlength=200),
	    firstname =	StringField(maxlength=200),
	    lastname =	StringField(maxlength=200),
	    password =	StringField(minlength=5),
	    bio =	StringField(default="That's me!"),
	    activation_code = StringField(),
	    password_code = StringField(),
	    activation_limit = DateField(),
	)

	# we have automatically added fields like _creation_date, _mod_date

	def init(self):
	    """initialize a new document type. This is only called if the _initialized flag is 
	    not set which the collection usually does."""

	def generate_password(self):
	    """automatically generate a password"""
	    return unicode(uuid.uuid4())


    class Users(Collection):
	"""a user collection"""

	document_type = User 



    user = User()
    user.firstname = "Foo"
    user.lastname = "Bar"
    user.email = "mail@example.org"
    user.generate_password()

    user = users.save(user)
    user.nickname = "foobar"
    user.save()









