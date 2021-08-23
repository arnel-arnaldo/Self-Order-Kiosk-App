# Section 7: Self Order Kiosk Like McDonald's: Use Context and Reducer Hooks

## Based on the uDemy online course: React: The Complete Guide (Hooks, Context, Redux & 5 Apps) by Bassir Jafarzadeh

### Section 7.3 [Create Choose Order Screen](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26278540)

Things implemented:

1. Create Choose Order Screen

   - created ChooseScreen.js component
   - called from HomeScreen.js component using <CardActionArea onClick={() => props.history.push('/choose')}>. To use history, needed to install **React Router DOM** (_$ npm install react-router-dom_). Also, in App.js, need to wrap return() with `<BrowserRouter></BrowserRouter>`.
   - created Routes in App.js for HomeScreen, ChooseScreen

2. Create React Context

   - created a file called Store.js

3. Create Set Order Type Action
4. Redirect User to Order Screen

### Section 7.4 [Create Backend API for Categories](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26342582)

1. Create server.js

   - this will use Node Express to act as simple server.
   - install Express (_$ npm install express_)
   - create a port to listen to (`process.env.PORT || 5000`)
   - install nodemon to run server (_npm install nodemon --save-dev_)
   - add a script in package.json to run both nodemon and node server.js

2. Create data.js with sample data

   - create categories object with array of name & image objects

3. Return categories
   - create a route in server.js (`app.get('/api/categories', ...`) to return categories

### Section 7.5 [List Categories in the Sidebar](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26342588)

1. Create Order Screen

   - create **OrderScreen.js** in src/screens

2. Get categoryList from context

   - add **listCategories()** function in action.js:

   ```
   export const listCategories = async (dispatch) => {
      dispatch({ type: CATEGORY_LIST_REQUEST })
      try {
         const { data } = await axios.get('/api/categories')
         return dispatch({
            type: CATEGORY_LIST_SUCCESS,
            payload: data,
         })
      } catch (error) {
         return dispatch({
            type: CATEGORY_LIST_FAIL,
            payload: error.message,
         })
      }
   }
   ```

3. List categories in use effect

   - in OrderScreen.js, add this before return():

   ```
   useEffect(() => {
    listCategories(dispatch)
   }, [dispatch])

   ```

4. Show categories in left side

   - in OrderScreen.js, add this on return():

   ```
   {categories.map((category) => (
      <ListItem button key={category.name}>
         <Avatar alt={category.name} src={category.image} />
      </ListItem>
   ))}

   ```

### Section 7.6 [Create Backend API for Products](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26342602)

1. Connect to MongoDB and use Mongoose
2. Create Product Model
3. Seed Products
4. Create API for Products
