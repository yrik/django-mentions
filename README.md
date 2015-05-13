django-mentions
===============

Twitter-like mentions for django

Add application to INSTALLED_APPS in **settings.py**:

    INSTALLED_APPS = (
        ...,
        'mentions',
    )

Add app urls to your **urls.py**:

    urlpatterns = patterns('',
        ...
        url(r'^mentions/', include('mentions.urls')),
    )

Implement a mention provider (in mentions.py module, for example):

    from mentions import Provider

    class UserProvider(Provider):
        model = User

        def get_title(self, obj):
            return obj.username

        def search(self, request, term):
            return self.get_queryset().filter(username__istartswith=term)

Add this provider to your **settings.py**:

    MENTIONS_PROVIDERS = {
        'default': [
            'accounts.mentions.UserProvider'
        ]
    }

Use `mentions.forms.MentionTextarea` widget instead of the default one:

    from mentions.forms import MentionTextarea

    class PostForm(forms.ModelForm):
        class Meta:
            model = Post
            widgets = {
                'text': MentionTextarea
            }

To urlize mentions in your templates, use `urlize_mentions` filter:

    {{ post.text|urlize_mentions }}
