# Data Scrape Live Project



## Introduction

For the two week sprint I have been working on a data scrape application with a small team for the Tech Academy. The application used the Django framework, which created a lot of learning opportunities since I did not have much exposure to Django before starting this project. The project was starting from scratch so there were many [back end stories](#back-end-stories), and [front end stories](#front-end-stories) to work on, all varying in degrees of difficulty. I had the chance to work with a API in one of my stories, which I was really excited to do since I didn't have much prior experience with APIs. Over the two week sprint I also had the opportunity to work on some other project management and team programming [skills](#other-skills-learned) that I'm confident I will use again and again on future projects.

Below are descriptions of a few stories that I felt most accomplished of, along with code snippets, screenshots and navigation links.



*Navigation: [Introduction](#Introduction), [Back End Stories](#back-end-stories), [Front End Stories](#front-end-stories), [Other Skills](#other-skills-learned), [Top of Page](#data-scrape-live-project)*

## Back End Stories

- [Directions App](#directions-app)

- [Messaging App](#messaging-app)

- [User Creation](#user-creation)

  

*Navigation: [Introduction](#Introduction), [Back End Stories](#back-end-stories), [Front End Stories](#front-end-stories), [Other Skills](#other-skills-learned), [Top of Page](#data-scrape-live-project)*

### Directions App

A story that I worked on was to create a app that would give directions from a starting point to a end point. I achieved this by using a Map Quest API, which came with a few struggles, but overall was a great experience for me to interact with a API. One of the problems I came across while working on the directions app was if the database had no previous start location, and end location the application's page would crash. I fixed this issue by wrapping the form for locations in a if/ statement, then the request to the API in a elif. If it was a GET request only the form would load, but if it was a POST request then the application would make a request to the API for the directions.

```
def directions(request):
    url = 'https://www.mapquestapi.com/directions/v2/route?key=qGyMswGafi2puTNqSP91ETXcNRDFrAyG&from={}&to={}&outFormat=json&ambiguities=ignore&routeType=fastest&doReverseGeocode=false&enhancedNarrative=false&avoidTimedConditions=false'

    if request.method == 'GET':
        form = RouteForm()
        context = {'form' : form}
        return render(request, 'Traffic/traffic.html', context)
        
    elif request.method == 'POST':
        form = RouteForm(request.POST)
        form.save()
        
        latest_direction = Route.objects.filter().order_by('-id')[0]
        r = requests.get(url.format(latest_direction.start, latest_direction.end)).json()   
        direction_list = []
        json_list = r['route']['legs']

        for i in json_list:
            for j in i['maneuvers']:
                direction_list.append(j['narrative'])

        route = {
            'from' : latest_direction.start,
            'to' : latest_direction.end,
            'directions' : direction_list,
        }

        context = {'route' : route, 'form' : form}
        return render(request, 'Traffic/traffic.html', context)
```



### Messaging App

How I created the messaging app was by creating a Message model that I used to create a model form from. The model form would take the inputs of a receiver and the message.



```
def message(request):
    if request.method == "POST":
        form = MessageForm(request.POST)
        if form.is_valid():
            message = form.save(commit=False)
            message.sender = request.user
            message.created_at = datetime.now()
            message.save()
            return redirect('inbox')
    else:
        form = MessageForm()
    return render(request, 'Chat/message.html', {'form': form})
```



The tricky part was creating a inbox that would only show messages that the user logged in could only see messages that were sent or received by that user. I was able to accomplish this by filtering the Messages table for entries of sender and receiver that the user logged in currently was in.



```
def inbox(request):
    messages = Message.objects.filter(Q(sender=request.user) | Q(receiver=request.user)).order_by('-created_at')
    return render(request, 'chat/inbox.html', {'messages': messages})
```



### User Creation

I was able to create a user creation system by using a built in user creation form that Django supplies. It is a scaffolded form for the User model Django has built in, so I created a SignUp class, and plugged in the form with a signup.html page to be rendered with it.

```
class SignUp(generic.CreateView):
    form_class = UserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'signup.html'
```





## Front End Stories

- [Directions Page](#directions-page)



*Navigation: [Introduction](#Introduction), [Back End Stories](#back-end-stories), [Front End Stories](#front-end-stories), [Other Skills](#other-skills-learned), [Top of Page](#data-scrape-live-project)*


### Directions Page

After creating the functionality of the directions app I wanted to make the page to look a lot cleaner and stick to the schema of the whole application. Our project was using the CSS framework bootstrap 4, so I implemented a few of there classe to the page.

![directions](https://user-images.githubusercontent.com/44681780/55760983-a3786b80-5a12-11e9-9b53-df6e017f5c73.PNG)





## Other Skills Learned

- Improving project work flow by communicating the team about merging conflicts
  - I resolved merging conflicts for the team 
- I gained new perspectives from my team members on how they learn new technologies
- Improved communication skills while working on a project in a team



*Navigation: [Introduction](#Introduction), [Back End Stories](#back-end-stories), [Front End Stories](#front-end-stories), [Other Skills](#other-skills-learned), [Top of Page](#data-scrape-live-project)*
