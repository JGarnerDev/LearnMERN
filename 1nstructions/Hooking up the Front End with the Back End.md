-------------------- Hooking up the Front End with the Back End --------------------

create-user

1. Axios

    1a. npm install axios

    1b. import

    1c. 

                                            onSubmit(e) {
                                                e.preventDefault();

                                                const user = {
                                                    username: this.state.username
                                                };

                                                console.log(user);

                                                axios
                                                    .post("http://localhost:5000/users/add", user)
                                                    .then(res => console.log(res.data));

                                                this.setState({
                                                    username: ""
                                                });
                                            }

2. Looking back to understand 

    2a. Go to our users.js in our backend directory in the 'routes' folder

                                            router.route('/').get((req, res) => {
                                                User.find()
                                                    .then(users => res.json(users))
                                                    .catch(err => res.status(400).json('Error: ' + err))
                                            })

                                            router.route('/add').post((req, res) => {
                                                const username = req.body.username

                                                const newUser = new User({username})

                                                newUser.save()
                                                    .then(() => res.json('User Added!'))
                                                    .catch(err => res.status(400).json('Error: ' + err))
                                            })

3. Test! 

    3a.

    3b.                           User Added!

    3c. Duplicates ----> Error

    3d. Check MongoDB!

4. Applying this to create-exercise

    4a. Import and axios.post

                                            import axios from 'axios'

                                            axios
                                                .post("http://localhost:5000/exercise/add", exercise)
                                                .then(res => console.log(res.data));

    4b. Look to back end to understand (exercises route)

    4c. Make changes to our componentDidMount()
                                


                                            componentDidMount() {
                                                this.setState({
                                                    users: ["test user"],
                                                    username: "test user"
                                                });
                                            }



                                                    Into...


                                            componentDidMount() {
                                                axios.get('http://localhost:5000/users/')
                                                .then(response => {
                                                    if(response.data.length > 0) {
                                                        this.setState({
                                                            users: response.data.map(user => user.username),
                                                            username: response.data[0].username
                                                        })
                                                    }
                                                })
                                            }

    4d. Explain the map function with MongoDB. 

    4e. Take a look at 'Create New Exercise' on our app! 

    4f. Look at MongoDB!

5. The exercise-list component (1:29)

    5a. Import and axios.post

                                            import { Link } from "react-router-dom";
                                            import axios from "axios";

                                            componentDidMount() {
                                                axios
                                                    .get("http://localhost:5000/exercises/")
                                                    .then(response => {
                                                        this.setState({ exercises: response.data });
                                                    })
                                                    .catch(error => {
                                                        console.log(error);
                                                    });
                                            }

    5b. Look to back end to understand (exercises route)

    5c. Making a 'delete exercise' method

                                            deleteExercise(id) {
                                                axios
                                                    .delete("http://localhost:5000/exercises/" + id)
                                                    .then(res => console.log(res.data));
                                                this.setState({
                                                    exercises: this.state.exercises.filter(el => el._id !== id)
                                                });
                                            } 

    5d. Our JSX

                                            render() {
                                                return (
                                                    <div>
                                                        <h3>Logged Exercises</h3>
                                                        <table className="table">
                                                            <thead className="thead-light">
                                                                <tr>
                                                                    <th>Username</th>
                                                                    <th>Description</th>
                                                                    <th>Duration</th>
                                                                    <th>Date</th>
                                                                    <th>Actions</th>
                                                                </tr>
                                                            </thead>
                                                            <tbody>{this.ExerciseList()}</tbody>
                                                        </table>
                                                    </div>
                                                );
                                            }

    5e. Add a method

                                            exerciseList() {
                                                return this.state.exercises.map(currentexercise => {
                                                    return <Exercise exercise={currentexercise} deleteExercise={this.deleteExercise} key={currentexercise._id} />
                                                })
                                            }