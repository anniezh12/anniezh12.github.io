---
layout: post
title:      "React/Redux App with Rails Api"
date:       2018-04-19 03:07:35 -0400
permalink:  react_redux_app_with_rails_api
---


 React is a JavaScript library for building user interfaces. Redux is an open-source JavaScript library for managing application state. It is most commonly used with libraries such as React or Angular for building user interfaces.
 I will briefly explain the concepts and features that I used in this app along with an explanation of the code. 
The concept starts with a web page being a collection of components (much like the states in our United States).
It is not only easy to manage components individually, but they can also be used in other parts of code as needed.

### ***Creating  A React App***

##### **step 1.**
In your terminal type

  A. create-react-app eventbright(will  create  a react app)
	
  B. npm/yarn start (will start it in the web browser)
	
  C. Go to App.js and put



```
import React, { Component } from 'react';

import './App.css';
import Event from './event.js'
import EventsFromApi  from './eventsfromapi.js';
class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to Event Be Right</h1>

        </header>
           <Event  />
           <EventsFromApi />
      </div>
    );
  }
}

export default App;
```

 D. Create a file in src/component/event.js( I createdtwo folders for keeping stateless components  seperate from components with state)
    
   Event component here will serve a  container component to hold EventForm component as well as a presentational component 
   EventsLists  
```

 import React,{Component} from 'react';
import EventForm from './eventform.js'

const EventsLists = ({events}) =>(

    <div>
    {events.map((event,index)=>{
      return (<li key={index}> {event.name}</li>)
      }
 )}
    </div>

);

export default class Event extends Component{

  constructor(props){
    super(props);
    this.state={
      events:[],//an empty array to hold future events
        }
  }

  addEvents = (event) =>{
   this.setState({
    events:[...this.state.events,event,]
   })
  }


  render()  {

    return( <div>
      <h4>You can add events here</h4>
      Number of Events Added :{this.state.events.length}
      <br/>
        <EventForm onSubmit={this.addEvents}/>
        <EventsLists events={this.state.events}/>
      </div>
    )

  }
}

```


once this component is rendered it calls two othercomponents 
  1. EventForm
  2. EventsLists

There are two kinds of React Components
1. Functional/stateless Components(Also known as Presentational Components)
2. Class Components ( It better to use them when there is need to use lifecycle methods or manipulation of state)

  
#### 1. EventForm(Class Component )
    when called and also  accepts a prop (addEvents method resides in Event component) 

```
import React,{Component} from 'react';
export default class EventForm extends Component{
  constructor(props){
    super(props);
    this.initialState={
      name:"",
      city:"",
      date:""
    }
    this.state = this.initialState;
  }
  handleChange=(event)=>{
    this.setState({
[event.target.name]: event.target.value //will assign all the input to its related state
    })
  }

   handleSubmit = (event) =>{
    event.preventDefault();
    this.props.onSubmit(this.state);
     }

  render()
  {
    return(
      <div>
         <form onSubmit={this.handleSubmit}>
         Name:<input type="text" name="name" value={this.state.name} onChange={this.handleChange}/><p/>
         City   :<input type="text" name="city" value={this.state.city} onChange={this.handleChange}/><p/>
         Date :<input type="text" name="date" value={this.state.date} onChange={this.handleChange}/><p/>
         <input type="submit" name="Add"/>
         </form>

      </div>
    )
  }
}
```

#### 2. EventsLists(Presentational Component)

Next EventLists component will be called which is a presentational component and this.state.events(current array of events) will be passed as props.
new events added by a user will be displayed.


the above component will render a form in front of a user and upon submission data will be added to an array events(residing in Event component)


### 2. EventsFromApi

This is is called after Event component is being rendered. this component actually takes benefit of some builtin component life Cycle methods 
for example omponentWillMount() will use a fetch method to fetch the user info from Eventbrite Api and once a component has been mounted it will use 
componentDidMount() to actually fetch the information of current user events information.

```
import React,{Component} from 'react';
import 'isomorphic-fetch';


const MY_TOKEN = "Token generated in eventbrite";
const URL=`https://www.eventbriteapi.com/v3/users/me/?token=${MY_TOKEN}`;

const CurrentUserEventsLists = ({events}) => (
  <div>
  {
    events.map((event,key) => {return (
                        <li key={key}><b> {event.name.text}</b>
                              <br/>{event.description.text}
                              <br/>Time: {event.start.local}
                              <br/> Time Zone :{event.start.timezone}
                              <br/> Price :{}
                              <br/>Tickets Left: {event.capacity}
                          </li>)
                        })

   }
  </div>
)

export default class EventsFromApi extends Component{

constructor(){
  super();
  this.state = {
    eventsByCurrentuser : [],
    user: ''

  }
}

componentWillMount(){
   // The following fetch will fetch from the Api and will get only current user info
    fetch(URL)
    .then(response => response.json())
    .then(res => this.setState({user: res.name}))
    }

  componentDidMount(){

    //The following fetch will fetch from the Api and will get only current user info
    fetch(`https://www.eventbriteapi.com/v3/users/me/owned_events/?token=${MY_TOKEN}`)
    .then(response => response.json())
    .then(resp => {this.setState({eventsByCurrentuser: resp.events})});

  }

  render()
  {
    return(
      <div className="bg-warning text-white" >
        <h4>All Events by {this.state.user}!</h4>

          <ol>
           <CurrentUserEventsLists events={this.state.eventsByCurrentuser}/>
           </ol>
      </div>
    )
  }
}

```

#### Using React Routers 
   
   Currently eventbright has only one index page which renders all components on it.
    The web is just a series of links to other pages hence we will at some points need to have more pages that a user can interact to, to do so 
     i will use React Routes. Enter React Router: a routing library for React that allows us to link to specific urls then show or hide various components depending on which url is displayed.
     
#### step 1 .
In the terminal enter  `npm install react-router-dom`

#### step 2. 
Inside index.js 

```
ReactDOM.render(
  <div>
          <Provider store={store}>
            <App store={store}/>
         </Provider>
  </div>,
  document.getElementById('root'));
```
#### step 3.
In App.js

```
Import react-router functions in index.js
         import React, { Component } from 'react';

import './App.css';
import Event from './event.js'
import EventsFromApi  from './eventsfromapi.js';
import { connect } from 'react-redux';
import { BrowserRouter as Router, Route , NavLink} from 'react-router-dom'; //to user react routes
import NavBar from './NavBar';
import Home from './Home';
import About from './About';
import ContactUs from './ContactUs'

class App extends Component {

   handleOnClick(){
     this.props.store.dispatch({
       type:'INCREASE_COUNT',
     })
   }

  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1 className="App-title">Welcome to Event Be Right</h1>
        </header>

                  <Router>
                   <div>
                   <NavBar/>
                      <Route exact path="/home" component={Home}/>
                      <Route  path="/about" component={About}/>
                      <Route  path="/contactus" component={ContactUs}/>
                   </div>
                 </Router>
           <Event  />
           <EventsFromApi />
           <button onClick={(e)=>this.handleOnClick(e)}>
               Click
           </button>[{this.props.store.getState().items.length}]
      </div>
    );
  }
}

const mapStateToProps = (state) =>{
  return {items: state.items}
}
export default connect(mapStateToProps)(App);

```
#### step 4.

defining NavBar component which will provide us links
```
import React from 'react';
import { NavLink } from 'react-router-dom';

/* Add basic styling for NavLinks */
const link = {
width:'100%',
  clear: 'both',
  padding: '10px',
  margin: '10 5 6px 6px',
  background: '#27AE60',
  textDecoration: 'none',
  color: 'white',
}

        const NavBar = () =>{
          return(
           <div className="link">
              <NavLink to="/home"  style={link}>Home  </NavLink>
              <NavLink to="/about"  style={link}>Events  </NavLink>
              <NavLink to="/event"  style={link} >React </NavLink>
              <NavLink to="/eventfromapi"  style={link}>Eventbrite </NavLink>
              <NavLink to="/redux"  style={link}>Redux </NavLink>
              <NavLink to="/contactus"  style={link}>Contact Us  </NavLink>
           </div>
         )
       }
       export default NavBar;

```

where NavLink is the one linking  buttons to the Routes in App.js
I also create Components with name Home,About and ContactUs which are being inported by NavBar.js

### Using Redux 

I want to be able to use Redux to pull all events that are stored in the data.js(containing some seed data)
In the console run
`npm install redux --save`

So far I have used react which is great in some ways but once an application starts becoming bigger and has many components,
managing different components with states and passing data from one component to another becomes harder.
Readux provides us methods and tools to manage all data in our application as a plain JavaScript object called state.
Redux make it easy to store all data on one place accessible to all of our components and also provides 
us with functions to manipulate this data hence updating the state when there is a change. This state is called store in redux.

#### Provider Store

As I coded earlier in th index.js file 

```
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import registerServiceWorker from './registerServiceWorker';
import eventsReducers from './reducers/eventsReducers'
import thunk from 'redux-thunk';

const store = createStore(eventsReducers,applyMiddleware(thunk));

ReactDOM.render(
  <div>
         <Provider store={store}>
            <App />
         </Provider>
  </div>,
  document.getElementById('root'));
registerServiceWorker();

```
First of all I imported `createStore` and `Provider` components from react and redux.

I have wrapped App component in a Provider component and passed store as props, which will actually 
make every component in the app tree to be able to access Redux store through "connect" method.
To be able to use Provider we need to import it
`import {Provider} from 'react-redux'`
Moreover store in index.js is being created with createStore function of Redux.
Our createStore function has two parameters first is a function called reducers which actually initialize our state at this point. But later reducer will be called with almost all the request we make to achieve CRUD actions.

```
export default function eventsReducers(state= {events:[]},action){

  switch(action.type)
  {
    case 'ADD_EVENT':
     
    case 'DISPLAY_EVENTS':
      
    case 'UPDATE_EVENT':

    case 'DELETE_EVENT':
       
    default:
      return state;

  }
}
```
so our reducer will set out initial state of our store to an empty array of events which will be populated with corresponding requests. A reducer uses a switch-case structure and its main purpose it to return updated state after each call.

Inside App.js
 I am using a route to display "EventDisplayedUsingRedux" component. And since I want to send props to this component I used render attribute which will actually 
render a component and will pass current store as a prop to it.
```
<Route  exact path="/redux" render={()=><EventDisplayedUsingRedux store={this.props.store}/>}/>
```

Now once clicked this link we will start getting errors because I havent wrote  `EventDisplayedUsingRedux` component  yet.
inside src/components/eventdisplayingredux.jswrite the following code
```


import React from 'react';
import { connect } from 'react-redux';
import {addEvent} from '../actions/addevent.js'

 class EventDisplayedUsingRedux extends React.Component{
 constructor(props){
   super(props);
   this.state={
     name:'',
     city:'',
     date:'',
   }
 }

  handleOnChange = (event) => {
    this.setState({
      [event.target.name]: event.target.value

    })
  }

 handleOnSubmit = (event) => {
   event.preventDefault();
   this.props.addEvent(this.state);
     }

  render(){

    const eventLists = this.props.events.map((el,i)=><li key={i}>{el.name}</li>);

    return(

      <div>
      {eventLists}
      <h3>Using Redux</h3>
      <div className="jumbotron">
            <form onSubmit={this.handleOnSubmit}>
            <div className="form-inline">

                  Name:<input type="text" className="form-control" name="name" placeholder="Name" value={this.state.name} onChange={(event)=>{this.handleOnChange(event)}}/>
                  <br/>
                  City:    <input type="text" className="form-control" name="city" placeholder="City" value={this.state.city} onChange={(event)=>{this.handleOnChange(event)}}/>
                  <br/>
                  Date:    <input type="text" className="form-control" name="date"  placeholder="mm/dd/yyyy" value={this.state.date} onChange={(event)=>{this.handleOnChange(event)}}/>
                  <br/>
                  <input type="submit"/>
                  </div >
            </form>
        </div>

      </div>
    )
  }
}

const mapStateToProps = (state) => {
  return { events: state.events };
   }


export default connect(mapStateToProps,{addEvent})(EventDisplayedUsingRedux);```



Ok so it looks pretty simple, there is a form  which on change of any field call a method which sets value of state to current inpput and upon submission we see a call to `this.props.addEvent(this.state)` what is this.props? 
Actually in redux once we connect our app with a store we can refer to the current state as this.props instead of this.state.
Moreover there is another function mapStateToProps all it does it returns a variable containg our current state so we can easily refer to it like this.props.events and finally
everything is connected at the bottom using a connect method. Now our redux store knows that there is a variable events, an array containg all events.

### ADDING RAILS API
  Before showing this function leta create a rails Api
	  In the terminal run
		
		`rails new EventsApi`
		`rails g controller events` (will create a controller and related models)
		inside models/event.rb I used some validations 
		
	
```
	class Event < ApplicationRecord
			validates :name, presence: true,uniqueness:true
			validates :city, presence:true
			validates :date, presence:true
  end

```




#### ADD EVENT

As we can see a call is made to a function addEvent, this is in our actions folder(create a folder name actions). Create a file name as addevent.js and write the following code 


```
import 'isomorphic-fetch';
  let URL = 'http://localhost:3000';
 export function addEvent(currentEvent) {
 	return (dispatch) => {
		return fetch(`${URL}/events`,{
		method: 'post',
		body:JSON.stringify(currentEvent),
		headers : {
			 'Accept': 'application/json',
	 'Content-Type': 'application/json'

		 }
		})
		.then(resp => {
					return resp.json()
				})
		.then(events => dispatch({
			 type: 'ADD_EVENT',
			 events: events})
		 )
}}
```


 display function
export function displayEvents(){
  return (dispatch) =>{
    return fetch('${URL}/events')
    .then(resp => resp.json())
    .then(events => dispatch({type: 'DISPLAY_EVENTS',events: events}))
    }
  }


Now try to click the link `Redux` and we will get a cors error as
`CORS error No 'Access-Control-Allow-Origin' header is present on the requested resource`
null

So if you have your frontend and backend on different domains you’ll need to allow CORS (cross-origin HTTP request).

### Enabling CORS(cross-origin HTTP request)


Inside Gemfile
gem 'rack-cors'

create a file cors.rb

Inside config/initializers/cors.rb write thefollowing code

```
Rails.application.config.middleware.insertbefore 0, Rack::Cors do
  allow do
    origins 'localhost:3000'
    resource '',
      headers: :any,
      methods: %i(get post put patch delete options head)
  end
end
```

#### Add Event related Reducer and  Events Controller Code

Inside reducer add the following code under case: ' ADD_EVENT'

  ```
case 'ADD_EVENT':

	return {...state,events:[...state.events,action.events]}
```

Related Controller Action will look like as follows


```
def create
			@event= Event.new(event_params)
			if @event.save
				render json: @event
			end
		 end
```

so out fetch request will send a request to the controller create function and will pass all form data to it, where a new event will be created and the json response contaiing this new event will be send back to the .then part after fetch where response will be converted to json and finally a dispatch request will be fired with a type of  'ADD_EVENT' and events array including the most recent record added.  EventLists
will be displayed show name of the event jusrt created.

### Display, Update and Delete Actions

Similarly we can display, delete and update actions 

reducers/eventReducers.js


```
export default function eventsReducers(state= {events:[]},action){

  switch(action.type)
  {
    case 'ADD_EVENT':

      return {...state,events:[...state.events,action.events]}

    case 'DISPLAY_EVENTS':

      return {...state,events:action.events}

    case 'UPDATE_EVENT':

     return {...state,events:action.events}

    case 'DELETE_EVENT':

         return {...state,events: action.events }

    default:

      return state;

  }
}

```




### controllers/events_controller.rb



class EventsController < ApplicationController

def index
	@events= Event.all
	render json:@events
 end

	def create
		@event= Event.new(event_params)
		if @event.save
			render json: @event
		end
	 end

	 def update
		 @event = Event.find(params[:id])
		 @event.update(event_params)
			 render json: Event.all
	 end

	 def destroy
		 @event = Event.find(params[:id])
		 if @event.destroy
			render json: Event.all
		 end
	 end

	 private
		def event_params
			params.require(:event).permit(:name,:city,:date)
		end
		end
		
					
					
#### 		actions/deleteevent.js
					
```
let URL = 'http://localhost:3000/events'
export default function deleteEvent(event,eventId){

return dispatch => {
	console.log(event.id)
	return fetch(`${URL}/${eventId}`,{
	 method: 'delete'})
	.then(resp => resp.json())
	.then(events=> dispatch({
		type:'DELETE_EVENT',
		events: events
	}))
}
}
```

#### actions/updateevent.js


```
import 'isomorphic-fetch';
let URL = 'http://localhost:3000/events'
export default function updateEvent(event){
console.log("resp",event.id);
  return dispatch => {

    return fetch(`${URL}/${event.id}`,{
     method: 'put',
     body: JSON.stringify(event),
     headers:{
       'Accept': 'application/json',
       'Content-Type':"application/json"
     }
      })
    .then(resp => console.log("pool=",resp))
    .then(events=> dispatch({
      type:'UPDATE_EVENT',
      events: events
    }))
  }
}
```

#### actions/displayevents.js

```
import 'isomorphic-fetch';
let URL="http://localhost:3000";
export function displayEvents(dispatch) {
  return  (dispatch) => {
    return fetch(`${URL}/events`)
    .then(resp=>{
      return resp.json()
     })
    .then(allEvents => dispatch({
      type: 'DISPLAY_EVENTS',
      events: allEvents }))
      .catch(errors => {
        console.log(errors)
      })
}}
```

> Github Link:https://github.com/anniezh12/EventBright




			






