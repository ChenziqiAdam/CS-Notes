# Python API Development - Comprehensive Course for Beginners
### 1. Virtual Environment (VENE)
- Isolation and Dependency Management: 
	isolated environments for specific Python projects, each with its own set of dependencies, i.e. exact Python version
- pip: install packages
	`pip install`
- Anaconda {[[Anaconda]]}
### 2. FastAPI Basic
- "Hello World"
	```Python
	from fastapi import FastAPI
	app = FastAPI() # Instantiation

	# The part below is called "route" or "path operation"
	@app.get("/")
	async def root():
	    return {"message": "Hello World"}
```
	- activate virtual environment: `conda activate fastapi`
	- run the live server: `uvicorn main:app
	- `@`: decorator
	- `get()`: HTTP method to get a request
	- `"/"`: URL path
	- `async`: Python keyword to define asynchronous(非同步的) functions and methods
- Q. what if the programmer modify the code and want to refresh the api?
	- `uvicorn main:app --reload`
- The order matters for the requests.
	- Example #1
		```Python
		@app.get("/")
		async def hello():
		    return {"message": "Hello World"}
		
		@app.get("/")
		async def post():
		    return {"message": "This`is a post"}
```
		- go to \http://127.0.0.1:8000 will only see `{"message": "Hello World"}`
	- Example #2
		```Python
		@app.get("/")
		async def hello():
		    return {"message": "Hello World"}
		
		@app.get("/posts")
		async def post():
		    return {"message": "This`is a post"}
```
		- go to \http://127.0.0.1:8000/posts will see `{"message": "This`is a post"}`
- `POST`
	- `POST` can send data to the API server but `GET` cannot. Both can request the server to send back some data.
	- Example: `Body`
		```Python
		from fastapi.params import Body
		from pydantic import BaseModel
		
		class Post(BaseModel):
			title: str
			content: str

		@app.post("/create")
		def create_posts(new_post: dict = Body(...)): # extract the body data from post
			return {"new_post": f"title {new_post['title']} content: {new_post['content']}"}
```
- Schema Validation with `pydantic`
	- specify the type of the input data
	- Example
		```Python
		from fastapi.params import Body
		from pydantic import BaseModel
		
		class Post(BaseModel):
			title: str
			content: str
			published: bool = True # by default if the user does not provide it
			rating: Optional[int] = None

		@app.post("/create")
		def create_posts(new_post: Post): # extract the body data and store in Post
			return {"new_post": f"title {new_post.title} content {new_post.content}"}
```
	- Each `pydantic` model has a funtcion `.dict`, which converts it into a dictionary.
### 3. CRUD
- **CRUD**: four main functions of applications
	- Create: `POST` `/posts`
	- Read: `GET` `/posts`or`/posts/:id`(to get to a specific post)
	- Update: `PUT/PATCH`(`PUT` changes every field while `PATCH` changes a specific field) `/posts/:id`
	- Delete: `DELETE` `/posts/:id`
	```Python
	
```