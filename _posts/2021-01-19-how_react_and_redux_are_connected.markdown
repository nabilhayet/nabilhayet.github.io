---
layout: post
title:      "How React and Redux are Connected!"
date:       2021-01-19 21:40:57 -0500
permalink:  how_react_and_redux_are_connected
---


The main purpose of using redux is to eliminate the repeative process of sending some information from the parent element to its child component. By Using redux, any child or related component knows if a change happens in parent component without receiving any props from the parent. To illutrate more, lets see how react works first.

```class BlogPost extends React.Component {
  render() {
    return (
      <div>
        <BlogContent articleText={"Dear Reader: Bjarne Stroustrup has the perfect lecture oration."}/>
      </div>
    )
  }
}```

```class BlogContent extends React.Component {
  render() {
    return (
      <div>
        {this.props.articleText}
      </div>  
    )
  }
}``` 

We have two classes over here. One is Blogpost and another one is Blogcontent. The Blogpost is sending a string to Blogcontent as a prop. The prop name is articleText. In the Blogcontent class, we can render the prop by using 'this.props.prop.name'. The prop name can be anything and over here its name is articleText. So, the summary is, the parent component Blogpost sends some info as props to child component Blogcontent. The child component access the info as this.props.props_name. 

Wouldn't it be great if we could change something in the parent component and the child would know the change without getting any info from the parent? The redux comes handy in this case. In Redux, we declare a global state in the top level component called App and all other componenets have access to that state. If we want to replace the above code with redux state, we have to understand first how the redux works? Let's see an example of redux code. 

```function createStore(reducer) {
  let state;
 function dispatch(action) {
    state = reducer(state, action);
    render();
  }``` ```function getState() {
    return state;
  };```  ```return {
    dispatch,
    getState
  };
};```
```function reducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREASE_COUNT':
      return { count: state.count + 1 };
      default:
      return state;
  }
}```
```function render() {
  let container = document.getElementById('container');
  container.textContent = store.getState().count;
};```
 
```let store = createStore(reducer)
store.dispatch({ type: '@@INIT' });
let button = document.getElementById('button');
 button.addEventListener('click', () => {
  store.dispatch({ type: 'INCREASE_COUNT' });
});``` 

We can see that, there is a variable declared at the end of the createStore function. The variable is  a reference to that function. The createStore function takes one argument which is the reducer function. By using that variable we will be able to access any function declared inside that createStore function or any other attributes. The reducer function is responsible for changing the state and return that updated state. Ther is also another function called render which displays the updated state. Inside createStore function we decalre a local state variable, a function called dispatch which receives argument of action, another function called getState which return the updated state. The return value of the createStore function is both of thoso functions dispatch and getstate.

Let's see how they are all connected! We saw before that the store variable points to the createStore function. So the variable can access anything inside that function. Using that variable we are able to access the dispatch function which is declared inside the createStore. We are invoking the dispatch function by giving an initial action as an argument. Inside that dispatch function, we call the reducer function outside the createStore. Although the dispatch is inside but the reducer is outside the createStore, the dispatch is still able to access the reducer as we sent the reducer to createStore as an argument.

The dispatch function calls the reducer by giving two arguments. One is the local state declared inside createStore and the other is the action that dispatch got as its argument. We also set the return value of the reducer function equal to state. The reducer then updates the state based on the action type. After the state gets updated, we call the render function. The render function calls a function named getState inside createStore to access the updated state. Finally, the render function displays the state on the DOM. 

Now, our main goal is to use global state for our first example.  Let's see how can we do this. In Index.js, we have the following code.

```const store = createStore(
  The Reducer Name,
);```
```ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);``` 

We saw before the store variable points to the createStore function and the function receives reducer as its argument. We attach the top level componenet named 'App' with root by wrapping it with the store variable. By doing this, we are allowing the App component to access the global state. In the App.js we have the following code 

```class App extends Componenet { 
         render() { 
				    return ( 
						           <BlogPost/>
											 <BlogContent/>
											 )
					}
		}```
		
Since, inside the App component we are calling BlogPost and BlogContent, they will also be able to access the store variable which points to createStore. 

In the BlogPost, We need to access the dispatch function so that we can send an action as an argument. To allow the BlogPost to access the global state and to call dispatch function, we use connect function. 

```export default connect(null, mapDispatchToProps)(BlogPost);``` 
The connect function takes in two arguments, first mapStateToProps and second mapDispatchToProps. The mapStateToProps returns the state and mapDispatchToProps takes the action passed from the class Componenet. 

We can not access the dispatch function directly. So we have to define that function outside the class. 

```const mapDispatchToProps = dispatch => {
  return {
      addArticle: () => { dispatch(addArticle(article) }
    }
}``` 


```class BlogPost extends React.Component {
  render() {
	   const article = 'Hello World'
    return (
      <div>
        {this.props.addArticle(article)}
      </div>
    )
  }
}```
const mapDispatchToProps = dispatch => {
  return {
      addArticle: () => { dispatch(addArticle(article) }
    }
}```
```export default connect(null, mapDispatchToProps)(BlogPost)```


So inside BlogPost, instead of sending props to BlogContent, we are invoking the mapDispatchToProps function by sending article variable as argument. Inside, mapDispatchToProps function, we call another function called addArticle by giving the previous argument. The addArticle function defined outside this class looks like this. 

```const addArticle = (article) => { 
      return { type: ADD_ARTICLE,
			                article: article 
			}```
	
The addArticle function returns an action. The action is an object. The object has two keys. One is type and another one is the variable that we sent from BlogPost. After getting back the action, the dispatch function inside the BlogPost class calls the reducer function. 
	
```export default function articleReducer(state = { articles: [] }, action) {
    switch (action.type) {
        case "ADD_ARTICLE":
            return {...state, articles: [...state.articles, action.article]};
						 default:
            return state;
    }
}``` 

The reducer function add the article to the state and returns the state. Now The state is Updated and the BlogContent class can access the state's value. 

```class BlogContent extends React.Component {
  render() {
	  const article =  this.props.articles.map(a => a.article) 
    return (
      <div>
       {article}
      </div>
    )
  }
}```
const mapStateToProps = state => {
  return {
       articles: state.articles
    }
}```
```export default connect(mapStateToProps)(BlogContent)```
											
At the end, the BlogContent access the updated state using this.props.state and renders the article on the DOM instead of getting the article as props from its parent BlogPost componenet. 

