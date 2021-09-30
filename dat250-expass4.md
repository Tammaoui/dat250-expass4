## Assignment 4.

Experiment 1 [x]

Experiment 2 [x]

Experiment 1: 

By running the app locally and calling the endpoint, I get this response:
![img.png](img.png)


Experiment 2:

#### Technical problems
- I did not encounter any problem. I cloned the project and ran the app and then ran the request as shown above.


#### Link to code
The code is in the Main class.

Code of interest is the following:



        get("/todo/:id", (req, res) -> {
            res.type("application/json");
            String id = req.params(":id");
            for (Todo todo : dummyDb) {
                if(todo.id == Long.parseLong(id)) {
                    return todo.toJson();
                }
            }
            return "No Todo with given Id found";
        });

        post("/todo/", (req, res) -> {
            res.type("application/json");
            Todo newTodo = new Gson().fromJson(req.body(), Todo.class);

            for (int i = 0; i < dummyDb.size(); i++) {
                if(dummyDb.get(i).id == newTodo.id)  {
                    return "Todo with this ID already exists.";
                }
            }
            dummyDb.add(newTodo);
            return newTodo.toJson();
        });

        delete("/todo/:id", (req, res) -> {
            res.type("application/json");
            String id = req.params(":id");
            for (int i = 0; i < dummyDb.size(); i++) {
                if(dummyDb.get(i).id == Long.parseLong(id)) {
                    dummyDb.remove(dummyDb.get(i));
                    return "Succesfully deleted";
                }
            }
            return "No Todo with given Id found";
        });

        put("/todo/:id", (req, res) -> {
            Todo toEdit = new Gson().fromJson(req.body(), Todo.class);
            boolean hasEdited = false;
            for (int i = 0; i < dummyDb.size(); i++) {
                if(dummyDb.get(i) == toEdit) {
                    // Edit this one.
                    Todo current = dummyDb.get(i);
                    if(toEdit.getDescription() != current.getDescription()) {
                        current.setDescription(toEdit.getDescription());
                        hasEdited = true;
                    }
                    if(toEdit.getSummary() != current.getSummary()) {
                        current.setSummary(toEdit.getSummary());
                        hasEdited= true;
                    }
                    if(hasEdited) return "Todo updated";
                }
            }
            return "Todo not updated";
        });

And the added code to Todo model:

    public String toJson () {
        Gson gson = new Gson();
        String jsonInString = gson.toJson(this);
        return jsonInString;
    }

I also used a ArrayList as a dummy storage to keep track of the todo items.
