Enhancing blog with Advanced features
---

    - send emails with django
    - create forms and handle them in views
    - create forms from models
    - integrating third-party applications
    - building complex Querysets
    
Sharing posts by email
---
follows this pattern

    - create a form for users to fill name/email, email recipient and optional comments.
    - Create a view in the views.py that sends the email
    - Add a URL for the new view in the URL.py file
    - Create a template to display the form

Creating forms with django
---

Django has a  built in form framework already set to go. 

    Form: allows us to build standard forms
    ModelForm: Builds forms tied to model instances
    
Create a Forms.py file inside directory of our blog. make it look like...

    from django import forms
    
    class EmailPostForm(forms.Forms):
        name = forms.CharField(max_length=25)
        email = forms.EmailField()
        to = forms.EmailField()
        comments = forms.charField(required=False, widget=forms.Textarea)

explanation

    The charField(required=False, widget=forms.Textarea) 
    defines our widget, to display as <textarea> instead of an <input>
    email and to fields are both Emailfields
    the comment field is optional with the required=false
    
Find out more at https://dics.djangoproject.com/en/2.0/ref/forms/fields/

    

Handling forms in views
---
Edit the views.py in blog

    from .forms import EmailPostForm
    
    def post_share(request, post_id)
        # Retrieve post by id
        post = get_object_or_404(Post, id=post_id, status='published')
        
        if request.method == 'POST':
            # FOrm was submitted
            form = EmailPostForm(request.POST)
            if form.is_valid():
                # form fields pass validation
                cd = form.cleaned_data
                # --- send email
            else: 
                form = EmailPostForm()
            return render(request, 'blog/post/share.html", {'post': post, 'form': form})
            
The post_share view takes the request object and the post_id variable

get_object_or_404() shortcut to retrieve the post by ID, and the post has publisehd status 

request.method == 'POST' to distinguish between GET and POST

GET method gets an empty form

POST method submits the form.

The Process is...

    1. With an new GET request, a form is will be used to display an emply form
    
        form = EmailPostForm()
    
    2. the user fills the form and submits via post
    
        if request.method == 'POST':
            # Form was submitted
            form = EmailPostForm(request.POST) 
            
    3. is_valid() method validates the data and returns True for all fields that contain valid data.
    4.
    5.
    


Sending emails with django
---

Rendering forms in templates
---

Creating a comment system
---

Creating forms from models
---

Handling modelforms in views
---

Adding comments to the post detail templates
---

Adding the tagging funtionality 
---

Retrieving posts by similarity
---

Summery
---

