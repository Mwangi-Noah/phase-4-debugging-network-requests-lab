# Putting it All Together: Client-Server Communication
## Your Notes Here

- Add a new toy when the toy form is submitted

- How I debugged: 
    -Error generated: (POST http://localhost:4000/toys 500 (Internal Server Error)). 
    
    This meant that during the toy creation process, there was an internal server error occurring in the Rails API. 
    The next logical step is to check the rails server logs usually displayed on the terminal running the rails server. According to the server logs, 
    -Error generated: (NameError (uninitialized constant ToysController::Toys)app/controllers/toys_controller.rb:10:in `create'). 
    
    This error message suggests that suggests that there is an issue with the naming of the Toy model or the ToysController class however the second potion of the error message point to the position where this problem arises. Looking at the create action, I found that there was a naming error (toy = Toys.create(toy_params)). Our model is Toy singular not plural. I changed this part and ran the create feature again which proved successful.


- Update the number of likes for a toy

  - How I debugged: 
    -Error generated: (Uncaught (in promise) SyntaxError: Unexpected end of JSON input). 
    
    Basically, this indicates that there is an issue with the JSON response being returned by the Rails API when updating the number of likes for a toy. 
    
    From our server logs, the update process for the number of likes seems to be successful. The response shows a 204 No Content status code, indicating that the update was processed without any content being returned in the response body. 
    
    This seems to be the problem so on checking the update action I found that there was nothing being rendered after the update was done.
   {def update
    toy = Toy.find_by(id: params[:id])
    toy.update(toy_params)
    #no rendered content
  end}. 
  I changed the response body by adding {render json: toy}. This seemed to work.


- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged: in this section, the chrome devtools generated the following error {ToyCard.js:7     DELETE http://localhost:4000/toys/1 404 (Not Found)}. This suggests that the server could not find the specified toy with the given ID. To debug and fix this issue, I first checked the server logs which defined this error {ActionController::RoutingError (No route matches [DELETE] "/toys/1")}. This meant that routing was the problem. In my route files, the only routes defined were {resources :toys, only: [:index, :create, :update]}. There was no route for destroy and once I added it, everything was working perfectly.
