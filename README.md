Review and Dive into Actions
==============

In this lesson, we will discuss:

  * The properties of an action
  * How to use action creators to create an action.

## Introduction

Actions are just plain javascript objects, but that doesn't mean we should ignore them.  In this section, we'll discuss the properties of actions, and how to use functions to create actions.

# Purpose of Actions

So as you know, we've been dispatching actions to our store to indicate the changes we would to make to our state.  In this way, actions almost feel like the request object or the parameters hash that you would see in a web application like ruby on rails.  

In rails, a user clicking on a link kicks off a request, and that request is ultimately passed to the controller, who then has the option of changing the database. In redux, a user may click on a button which dispatches an action, and the reducer would take information from that action to change the state.  You saw in the last section that simply by placing a console.log in our reducer, we could see a history of every action that was passed to the reducer, making our debugging job easier.

# Structuring Actions

Now an action is simply a plain javascript object that has a property of type.  The reducer uses this type property to see what it should do.  Here is an example of a valid action:

    const increaseCount = {type: 'INCREASE_COUNT'}

Now one can simply dispatch this action, for it to be handled by the reducer.

    store.dispatch(increaseCount)

Remember that the store has a dispatch action, which passes the action to the reducer, who then runs its switch state to decide what to do.

    function dispatch(action){
      reducer(state, action)
    }

    function reducer(state = {count: 0}, action){
        switch (action.type) {
          case 'INCREASE_COUNT':
            return {count: state.count + 1}
          default:
            return state;
        }
      }
# Action Creators

Ok, now we know that our actions are simply a plain javascript object with at least one property called type.  And example of using our actions is store.dispatch({type: 'INCREASE_COUNT'}).  Well what if we do the following.

  function increaseCount(){
    return {type: 'INCREASE_COUNT'}
  }

  store.dispatch(increaseCount())

Ok, so in the above lines of code we define a function called increaseCount whose job it is to return a an action.  Then we execute the increaseCount function, who returns that action, and we dispatch that action to the store.  If you think that this is equivalent of store.dispatch({type: 'INCREASE_COUNT'}), you are right.  

We prefer wrapping our actions in a function, because oftentimes our actions have some parts that will need to change, and a function comes in handy.  For example:

  function addTodo(todo){
    return {type: 'ADD_TODO', payload: todo}
  }

So in the above function, we can imagine generating actions with different payload properties depending on what we pass to to the addTodo function.

    addTodo('buy groceries')
      -> {type: 'ADD_TODO', payload: 'buy groceries'}
    addTodo('watch netflix')
        -> {type: 'ADD_TODO', payload: 'watch netflix'}

So essentially by wrapping our action in a function, we are able to easily keep some of the action properties the same, like type, and change others.  We would dispatch the action in the following way:

  store.dispatch(addTodo('buy groceries'))

That would return the action {type: 'ADD_TODO', payload: 'buy groceries'}, which we then send to the dispatch function.  
