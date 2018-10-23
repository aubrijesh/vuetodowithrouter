<template>
	<div class="app-body">
		<div class="create-app-panel">
			<div class="header">
				<div class="tab-list">
					<div class="tab all" v-on:click="filterTabSelected('all')" v-bind:class="{ active: filtered_todo_type == 'all'}">
						All
					</div>
					<div class="tab" v-bind:class="{ active: filtered_todo_type == tag, red: tag == 'red', green: tag == 'green', blue: tag == 'blue'}"  v-for="tag in tags" v-on:click="filterTabSelected(tag)">
						{{tag}}
					</div>
				</div>
			</div>

			<h4> Create New Todo</h4>
			<div class='todo-type-list'>
				<div class="todo-type" v-bind:class="{ active: todo_type == tag, red: tag == 'red', green: tag == 'green', blue: tag == 'blue'}"  v-for="tag in tags" v-on:click="todoTypeChanged(tag)">
					{{tag}}
					<input type="radio" value="red" name="todo-type" v-model="todo_type">
				</div>
			</div>
			<div class="todo-name">
				<input class="todo-name-input" type="text" placeholder="Enter todo name" v-model="todo_name" />
			</div>
			<div class='actions'>
				<button class="create-todo" v-on:click="createTodo()">Create Todo</button>
			</div>
			<div class="error-message" v-bind:class="{show: is_error}">{{error_message}}</div>
		</div>
		<div class="todo-item"  v-for="todo in filteredTodoList" v-on:click="openTodo(todo.id)">
			<div class="todo-name">
				{{todo.name}}
			</div>
			<div class="todo-tag" v-bind:class="{red:todo.tag == 'red', green:todo.tag == 'green', blue:todo.tag == 'blue'}">
				
			</div>
		</div>
	</div>
</template>

<script>
export default {
  name: 'Home',
  mounted: function() {
  	var existingTodoList = localStorage.getItem("todo_list");
  	if(existingTodoList) {
  		this.todo_list = JSON.parse(existingTodoList)
  	}
  },
  data: function() {
  	return {
  		todo_name: "",
	  	is_error: false,
	  	error_message: "",
	  	filtered_todo_type: "all",
	  	todo_type: "red",
	    tags: ["red", "green", "blue"],
	    todo_list: []
  	}
  	
  },
  methods: {
  	todoTypeChanged: function(type) {
  		this.todo_type = type
  		console.log("todo type changed");
  	},
  	createTodo: function(event) {
  		console.log("create todo is called");
  		if(this.todo_type == "") {
  			this.is_error = true;
  			this.error_message = "please select todo type";
  		}
  		else if(this.todo_name == "") {
  			this.is_error = true;
  			this.error_message = "please enter todo name";
  		}
  		else {
  			var newTodo = {
  				id: this.todo_list.length,
  				name: this.todo_name,
  				tag: this.todo_type,
  				description: ""
  			};
  			this.todo_list.push(newTodo);
  			this.todo_name = "";
  			localStorage.setItem("todo_list", JSON.stringify(this.todo_list))
  		}
  	},
  	openTodo: function(todoId) {
  		this.$router.push({ name: 'Details', params: { id: todoId }})
  	},
  	filterTabSelected: function(filterType) {
  		this.filtered_todo_type = filterType;
  	}
  }
  ,
  computed: {
    // a computed getter
    filteredTodoList: function () {
      var todoList = this.todo_list;
      var self = this;
      if(this.filtered_todo_type != "all") {
      	todoList = todoList.filter(function(obj, index) {
      		return obj.tag == self.filtered_todo_type;
      	});
      }
      return todoList;
    }
  }
 
}
</script>
<style lang="less" scoped>
@border-color: #e1e1e1;
@color-green: #3da51d;
@color-red: #bb1414;
@color-brown: #000000;
@color-blue: #2e7acc;


@heading-font-size: 16px;
@text-font-size: 14px;

.app-body {
		.header {
			.tab-list {
				width: 100%;
				display: flex;

				.tab {
					padding: 10px;
					color: white;
					width: 33%;
					cursor: pointer;
					transition: all .5s ease-in;
					&.active {
						padding: 12px;
					}
					&.all {
						background-color: @color-brown;

					}
					&.red {
						background-color: @color-red;

					}
					&.green {
						background-color: @color-green;

					}
					&.blue {
						background-color: @color-blue;

					}
				}
			}
		}

		.create-app-panel,
		.todo-item {
			float: left;
			padding: 20px;
		}
		.create-app-panel {
			width: 75%;
			height: 400px;

			.todo-type-list {
				display: flex;
				justify-content: space-between;
				align-items: center;
				max-width: 200px;

				.todo-type {
					padding: 5px;
					color: white;
					width: 33%;
					cursor: pointer;
					transition: all .2s ease-in;

					&.active {
						padding: 7px;
					}

					& > input[type=radio] {
						opacity: 0;
						width: 0;
						height: 0;

					}
					&:first-child {
						border-top-left-radius: 3px;
						border-bottom-left-radius: 3px;
					}
					&:last-child {
						border-top-right-radius: 3px;
						border-bottom-right-radius: 3px;
					}
					&.red {
						background-color: @color-red;

					}
					&.green {
						background-color: @color-green;

					}
					&.blue {
						background-color: @color-blue;

					}
				}
			}
			.todo-name {
				margin-top: 50px;
				input {
					width: 200px;
					padding: 10px 15px;
					outline: none;
					box-shadow: none;
					border: 1px solid @border-color;
				}
			}
			.actions {
				margin-top: 20px;

				.create-todo {
					padding: 10px;
				    outline: none;
				    border: 1px solid #e1e1e1;
				    background-color: #3da51c;
				    color: white;
				    border-radius: 3px;
				    cursor: pointer;
				}


			}
			.error-message {
				display: none;
				color: @color-red;
				font-size: 16px;
				margin-top: 20px;

				&.show {
					display: block;
				}
			}
			
		}
		.todo-item {
			width: 25%;
		    height: 200px;
		    float: left;
		    border: 1px solid #e1e1e1;
		    position: relative;
		    cursor: pointer;

		   	.todo-name {

		   	}

		    .todo-tag {
		    	width: 30px;
			    height: 30px;
			    position: absolute;
			    right: 5px;
			    top: 5px;
			    border-radius: 50%;
			    border: 5px solid red;
			    &.red {
					border-color: @color-red;
				}
				&.green {
					border-color: @color-green;
				}
				&.blue {
					border-color: @color-blue;
				}
			}
		}	
	}

</style>