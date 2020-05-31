# Advanced Internet Applications – laboratory - ASP.NET MVC

**Description :** The aim of the exercice was to familiaze with ASP.NET MVC. For this, I followed the following tutorial from Microsoft : https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-3.1  
In this description, I won't describe the code because everything is already well described in the tutorial. I'll first answer to the questions, then explain the problems I met, finally I will explain my point of view about this technology, with what I learn.

## What  are  the  responsibilities  of  each  layer  of  the  MVC  architecture  and  how  are  they connected?

The MVC architecture is composed of 3 layers : **M**odels, **V**iews and **C**ontrollers. Why is it composed like that? Well the answer is quite easy, it's to make the code cleaner, easier to test, but also (and for me it's the main reason) to make the code a lot more easier to maintain. It's easier to maintain since there is a "structure" in your code. For almost every PHP users, their first project was a hot mess since all code was "mixed". If one day, another developer want to work on one of these "mess" projects, he will spend a lot of time to know what is what, where is a sample of code... Here, all the project is well organized and make the life easier.
Before I write a novel to explain why MVC architecture is good, I'll explain each layer of the architecture :
* **Model** : It's classes which represent the data in the database. Each class represents a database table. In these classes, we can add "validation logic" to be sure we repect some rules about specific data (for instance a price must be between 10zl and 50zl).
* **Views** : It's components which display the User Interface(UI). Some views are related to model data in order to diplay data.
* **Controllers** : It's the component which represents the "functions" of the application, which will retrieve and manage data from the **Model**, then display the data with appropriate view template in the UI. In other words, it's the intermediary between the **Model** and **View** components. In ASP.NET MVC, the controllers also handle browser requests, we don't need to define a router (such as with Laravel for example). We can say it's the controllers which interact with user by displaying the data and view template wanted by the user following his actions.

In summary : Model represents data, view represents User Interface, and Controller is the component which interact with user by displaying what the user want (by using model and view).

## What  are  the  naming  conventions  for  models,  controllers,  controller  actions,  views  folders and views themselves?

* For Model : Its name must be similary to the name of the represented database table (the same would be ideal). **Warning** : The name of the model must be singular. For example in the tutorial, we called the model *Movie* because the table's name was *Movie*. It would still been *Movie* if the table's name was *Movies*.
* For Controller : It always end by "Controller". Generally (it's not a convention), it has the model name ending by "Controller". In our tutorial, the name of our controller was *MoviesController*.
* For Views folders : Each views folders must correspond to one controller. In our tutorial, for "*MoviesController*", we have "*/Movies*" folder inside "*/Views*" folder.
* For Controller actions and views : They must have the same name. When a controller method returns a view, by default it will use the view template which has the same name that the method. In our tutorial, the method *Index()* will return the view *Index.cshtml*.

## How to pass data from controllers to views (2 options)?

* By using ViewData dictionary, which is a dynamic object. We can access it as following :

```
ViewData["NumTimes"] = numTimes; //in controller
@for (int i = 0; i < (int)ViewData["NumTimes"]; i++) //in view 
```

* By specifing the type of object that the view expects. We can define it as following :
```
@model MvcMovie.Models.Movie // generally the 1st line of the view file
```
Here we expect a Movie object in our view. The controller must return the view with the expected object :
```
//in a controller method
var movie = await _context.Movie
        .FirstOrDefaultAsync(m => m.Id == id);
    if (movie == null)
    {
        return NotFound();
    }

    return View(movie);//here we return the view with an object
```

## How to map url’s to controller actions?

By default, the routing is set as following : *"{controller}/{action}/{id}"*. So we map to controller actions by using **localhost:{PORT}/Controller/actions**. If the actions require parameters, we can either use the questions mark "?" such as "*https://localhost:{PORT}/controller/action?name=Rick&numtimes=4*", or by using the *id* parameter such as "*https://localhost:{PORT}/controller/action/5*".  

## How  to  restrict  controller  actions  to  be  executed  only  via  certain  HTTP  request  types  (e.g., only via POST)?

We can use **HttpPost** attribute to specifiy that a specific action method can be invoked only for POST requests. We can also use **HttpGet** attribute but it's not necessary since **HttpGet** is the attribute by default.

## How to make sure a controller action can only be called through a form on our website and not through some external request?

By using **ValidateAntiForgeryToken** attribute. It works with an anti-forgery token generated by Form tag helper :
```
//in the begining of action code (in controller) :
[ValidateAntiForgeryToken]

//In the view
<form asp-action="Edit">//a simple example, as you can see, we won't need to write any code to use helper, only write the form tag is enough
```
When you will submit your form, the helper will generate an anti-forgery token that will be validated by the **ValidateAntiForgeryToken** attribute. If the **ValidateAntiForgeryToken** attribute is not validate, then it means it's a call from some external request and then it will return null (or an error, but it won't execute the action).
## Where do you define data validation and how do you ensure it in views and controllers?

Such as I already said in the first question, we define the validation in the model. In the controller we can ensure it with the *IsValid()* method. This method will evaluate any validation attributes that have been applied to the object. In the view, we have the following which will check the validation for each input :
```
@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```
Of course, we add this line for each input in the form :
```
 <span asp-validation-for="Title" class="text-danger"></span> // Replace the "Title" by the correct name
 ```
 If the input is not valid, it will display the error with the rule of validation for the input.
## Problems met
Even by only following a tutorial, I met a problem : In this step, https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-3.1&tabs=visual-studio, when you must create a controller with the CRUD methods & Views, I didn't have the functionnality in visual studio code. I solved this problem by installing the following packages : "Microsoft.AspNet.Mvc" and "Microsoft.AspNet.WebPages".

## Conclusion

I don't know if my laptop drinks redbull this time, but visual studio 2019 was a lot faster compared to usual. And honnestly, I begin to appreciate this technology. There is a lot of tools which makes the developer life easier, and the interface of visual studio (when it's not slow) is very pleasant. In this lab, I improved my knowledge about ASP.NET technology. As you can guess, I already knew the MVC architecture before doing this lab, but it's still useful to learn a new technology.  
I thank my teacher, sir Piernik, for this exercice and new skills acquired.
