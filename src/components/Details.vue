<template>
  <div class="todo-details">
  	<h3 class="main-heading">Todo Details</h3>
    <div class="todo-id row">
    	<h4>Id</h4>: {{todo.id}}
    </div>
     <div class="todo-name row">
     	<h4>Name</h4>: {{todo.name}}
    </div>
    <div class="todo-tag row">
  		<h4>Tag</h4>: <span class="tag-name" v-bind:class="{red:todo.tag == 'red', green:todo.tag == 'green', blue:todo.tag == 'blue'}">{{todo.tag}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Details',
  mounted: function() {
  	console.log("mounted")
  	var todoId = this.$route.params.id;
  	var todoList = [];
  	if(localStorage.getItem('todo_list')) {
  		todoList = JSON.parse(localStorage.getItem('todo_list'));
  	};
  	var filteredTodo = todoList.filter(function(obj, index) {
  		return obj.id == todoId;
  	});
  	if(filteredTodo.length > 0) {
  		this.todo = filteredTodo[0];
  	}

  },
  data () {
    return {
      todo: {}
    }
  }
}
</script>
<style lang="less" type="text/css" scoped>
	.main-heading {
		text-align: center;
	}
	.row {
		padding: 10px;
		margin-top: 10px;
		border-bottom: 1px solid #e1e1e1;
	}
	h4 {
		margin: 0;
		display: inline-block;
	}
	.tag-name {
		padding: 5px;
		border-radius: 2px;
		color: white;
		min-width: 100px;
		display: inline-block;
		text-align: center;
		text-transform: uppercase;
	}
	.tag-name.red {
	  background-color: #bb1414;
	}
	.tag-name.green {
	  background-color: #3da51d;
	}
	.tag-name.blue {
	  background-color: #2e7acc;
	}
</style>
