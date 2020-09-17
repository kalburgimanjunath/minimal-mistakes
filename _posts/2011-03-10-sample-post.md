---
layout: post
title: Redux
excerpt: "Redux"
modified: 2013-05-31
tags: [intro, beginner, jekyll, tutorial]
comments: true
---
State management is absolutely critical in a Web Application Development. 

We have discussed about how we manage state in a react application using State and Context. 

State contains data specific to a given component that may change over time. 

Using Context, we pass the data from parent component to Child component and from Child to Parent which are placed at different nesting levels.

For low-frequency updates like locale or theme changes or user authentication, the React Context is perfectly fine. But with a more complex state object like products in the shopping cart which has high-frequency updates, the React Context won't be a good solution. Because, the React Context will trigger a re-render on each update, and optimizing it manually can be really tough. 

Redux provides a solid, stable and mature solution to managing state in your React application.


 
If we have components that are siblings and need to share data, the way to do that in React is to pull that data up into a parent component and pass it down with props.

That can be cumbersome though. Redux can help by giving you one global “parent” where you can store the data, and then you can connect the sibling components to the data with React-Redux.

In this article, we will start understanding Redux.

Lets Open Index.js file from our demo-project. 

Add a New Javascript file in our src folder and we will name it as Employee.

I have the Contents of this Component handy and I Paste it here. 

 
We will create a new Component in our index.js file called AppComponent and we will call our Employee Component from AppComponent. 

We will render the AppComponent to our ReactDOM. 

Save the Changes, navigate to the browser. 

We can see that Employee Salary gets changed as we click on increase or decrease buttons. 

To start understanding Redux, we will re-write the same example using redux so that one can understand the basics of redux so well before we get into complex examples. 

We will create a new Component in our index.js file called AppComponent and we will call our Employee Component from AppComponent. 

We will render the AppComponent to our ReactDOM. 

Save the Changes, navigate to the browser. 

We can see that Employee Salary gets changed as we click on increase or decrease buttons. 

To start understanding Redux, we will re-write the same example using redux so that one can understand the basics of redux so well before we get into complex examples. 


 
 Lets install redux into our project by running a command 

Npm install –save redux react-redux

redux gives you a store, and lets you keep state in it, and get state out, and respond when the state changes. But that’s all it does.

It’s actually react-redux that lets you connect data of the state to React components.


 
The redux library can be used outside of a React app too. It’ll work with Vue or Angular apps as well.

Lets create one Global store using Redux where we are going to keep our data. 

Redux comes with a handy function that creates stores, and it’s called createStore.

To this createStore, We have to provide a function that will return the state data based on the action user performs. This function takes two parameters, one is the state data and the other one is action. That function is called a reducer in Redux terminology.


 
In short, reducer is a function that takes the current state, and an action, and returns the newState

We can create an employee object and pass that to the state parameter. 

Action is a JavaScript object. Action will have a type property that indicates the type of action being performed. Types should typically be defined as string constants.

We will write switch case on the action type and accordingly increment or decrement the salary. 

And we return the state object.

Now it’s time to hook up Redux to React.

To do that, the react-redux library comes with 2 things: a component called Provider, and a function called connect.

By wrapping the entire app with the Provider component, every component in the app tree will be able to access the Redux store if it wants to.


 
After this, Employee Component, and children of Employee, and children of their children, and so on – all of them can now access the Redux store.

But not automatically. We’ll need to use the connect function on our components to access the store.

Lets go to Employee.js file, 

Lets remove the state object and setstate calls we are doing. 

We can access the salary from the redux store using the props. 

When user clicks on increment or decrement button, we initiate the state change trigger by calling dispatch method. To the dispatch method, we pass the type.

We do the same in decrement function as well. 

To get the Salaryout of Redux, we first need to import the connect function.

Then we need to “connect” the Employee component to Redux at the bottom.

To the connect function, we pass another function as a parameter and that function takes state object as parameter and we can return the salary. 

Connect is a higer order Component to which we pass our Employee Component

Save the Changes, navigate to the browser. 

We can see that Employee Salary gets changed as we click on increase or decrease buttons. 

I hope we are clear now on what is action, reducer and store in redux. 

We will continue our discussion in our next article. 

Index.js

import React from 'react';
import ReactDOM from 'react-dom';
import Employee from './Employee';
import {createStore} from 'redux';
import {Provider} from 'react-redux';

const employeeData={
  salary:15000
};

const reducer=(state=employeeData,action)=>{
  switch(action.type){
    case 'INCREMENT':
      return {salary:state.salary + 1000};

    case 'DECREMENT':
      return {salary:state.salary - 1000};

    default:
      return state;
  }
}
const store=createStore(reducer);

const App = () => (
  <Provider store={store}>
    <Employee></Employee>
  </Provider>
);

ReactDOM.render(<App></App>,document.getElementById("root"));
Employee.js


/* eslint-disable no-useless-constructor */
import React from 'react';
import {connect} from 'react-redux';

class Employee extends React.Component {
    
    constructor(props){
        super(props);
    }

  incrementSalary = () => {
    this.props.dispatch({type:'INCREMENT'});
  }

  decrementSalary = () => {
    this.props.dispatch({type:'DECREMENT'});
  }

  render() {
    return (
      <div>
        <h2>Welcome to Employee Component...</h2>
        <div>
            <p>
                <label>Employee Salary : <b>{this.props.salary}</b></label>
            </p>
          <button onClick={this.incrementSalary}>Increment</button>          
          <button onClick={this.decrementSalary}>Decrement</button>
        </div>
      </div>
    )
  }
}

function mapStateToProps(state){
    return{
        salary:state.salary
    };
}
export default connect(mapStateToProps)(Employee);
