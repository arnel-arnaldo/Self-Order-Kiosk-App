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
   - install Mongoose: `$ npm install mongoose`
   - on **server.js** add the following lines:
     - `const mongoose = require('mongoose')`
     - `const dotenv = require('dotenv')` - environment variable for mongoose URL in .env (`MONGODB_URI=mongodb://localhost/selforderkiosk`)
     - install **dotenv** to access **.env**: `$ npm install dotenv`
     - **<ins>NOTE:</ins>** the `useCreateIndex: true` on `mongoose.connect()` is causing an error; removing it resolves it
2. Create Product Model
   - add this on server.js:
     ```
     const Product = mongoose.model(
     'products',
     new mongoose.Schema({
        name: String,
        description: String,
        image: String,
        price: Number,
        calorie: Number,
        category: String,
     })
     )
     ```
3. Seed Products
   - add this on server.js:
   ```
   app.get('/api/products/seed', async (req, res) => {
      const products = await Product.insertMany(data.products)
      res.send({ products })
   })
   ```
4. Create API for Products
   - add this on server.js:
   ```
   app.get('/api/products', async (req, res) => {
      const { category } = req.query
      const products = await Product.find(category ? { category } : {})
      res.send(products)
   })
   ```
5. Add the following **middleware** to read the content of `req.body` in the following added API and save it to MongoDB:

   ```
   app.use(express.json())
   app.use(express.urlencoded({ extended: true }))

   app.post('/api/products', async (req, res) => {
      const newProduct = new Product(req.body)
      const savedProduct = await newProduct.save()
      res.send(savedProduct)
   })
   ```

### Section 7.7 [List Products on Order Screen](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26342666)

1. Get productList from context
   - add **onClick()** on Category button, i.e. `onClick={() => categoryClickHandler(category.name)}`
     ```
     const categoryClickHandler = (name) => {
        setCategoryName(name)
        listProducts(dispatch, categoryName)
     }
     ```
2. List products in useEffect
   - whenever category changes, it will trigger the useEffect:
     ```
     useEffect(() => {
        if (!categories) {
           listCategories(dispatch)
        } else {
           listProducts(dispatch, categoryName)
        }
     }, [categories, categoryName, dispatch])
     ```
3. Show products in main section

### Section 7.8 [Add and Remove Food to Order](https://www.udemy.com/course/react-the-complete-guide/learn/lecture/26342670)

1. Create add to order modal
   - start with adding a `<Dialog>...</Dialog>` modal component in OrderScreen.js
2. Create addToOrder action
   - these codes handle it:
   ```
   <Button
      onClick={addToOrderHandler}
      variant='contained'
      color='primary'
      size='large'
      className={styles.largeButton}
      >
      ADD To Order
   </Button>
   ```
3. Create removeFromOrder action
   - these codes handle it:
   ```
   <Button
      onClick={cancelOrRemoveFromOrder}
      variant='contained'
      color='secondary'
      size='large'
      className={styles.largeButton}
      >
      {orderItems.find((x) => x.name === product.name)
         ? 'Remove From Order'
         : 'Cancel'}
   </Button>
   ```
4. Create my order section
   - these codes handle it:
   ```
   <Box className={[styles.bordered, styles.space]}>
      My Order - {orderType} | Tax: ${taxPrice} | Total: ${totalPrice} |
      Items: {itemsCount}
   </Box>
   ```
5. Handle cancel order
   - these codes handle it:
   ```
   <Button
      onClick={() => {
         clearOrder(dispatch)
         props.history.push(`/`)
      }}
      variant='contained'
      color='secondary'
      className={styles.largeButton}
   >
      Cancel Order
   </Button>
   ```
6. Handle finish order
   - these codes handle it:
   ```
   <Button
      onClick={previewOrderHandler}
      variant='contained'
      color='primary'
      disabled={orderItems.length === 0}
      className={styles.largeButton}
   >
      Done
   </Button>
   ```
