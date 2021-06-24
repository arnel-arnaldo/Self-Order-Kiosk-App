## Section 7: Self Order Kiosk Like McDonald's: Use Context and Reducer Hooks

### Based on the uDemy online course: React: The Complete Guide (Hooks, Context, Redux & 5 Apps) by Bassir Jafarzadeh

**Section 7.3** [Create Choose Order Screen](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26278540)

Things implemented:

1. Create Choose Order Screen

- created ChooseScreen.js component
- called from HomeScreen.js component using <CardActionArea onClick={() => props.history.push('/choose')}>. To use history, needed to install **React Router DOM** (_$ npm install react-router-dom_). Also, in App.js, need to wrap return() with <BrowserRouter></BrowserRouter>.
- created Routes in App.js for HomeScreen, ChooseScreen

2. Create React Context

- created a file called Store.js

3. Create Set Order Type Action
4. Redirect User to Order Screen
