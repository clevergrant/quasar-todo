<template>
	<q-page class="bg-grey-3 column">
		<div class="row q-pa-sm bg-primary">
			<q-input class="col" square filled bg-color="white" v-model="newTask" placeholder="Add Task" dense @keyup.enter='addTask'>
				<template v-slot:append>
					<q-btn round dense flat icon="add" @click='addTask' />
				</template>
			</q-input>
		</div>
		<q-list class="bg-white" separator bordered>
			<q-item
				v-ripple
				v-for='(task, index) in tasks'
				:key='task.title'
				@click='task.done = !task.done'
				clickable
				:class='{ "done bg-indigo-1" : task.done }'
			>
				<q-item-section avatar>
					<q-checkbox color="primary" v-model='task.done' class="no-pointer-events" />
				</q-item-section>
				<q-item-section>
					<q-item-label>{{ task.title }}</q-item-label>
				</q-item-section>
				<q-item-section
					v-if="task.done"
					side>
					<q-btn flat round dense color="negative" icon="delete" @click.stop='deleteTask(index)' />
				</q-item-section>
			</q-item>
		</q-list>
		<div class="no-tasks absolute-center" v-if="tasks.length <= 0">
			<q-icon name='check' size='100px' color="primary"></q-icon>
			<div class="text-h5 text-primary text-center">
				No Tasks
			</div>
		</div>
	</q-page>
</template>

<script>
export default {
	data: () => ({
		newTask: ``,
		tasks: [
			{
				title: 'Eat Breakfast',
				done: false,
			},
			{
				title: 'Eat Lunch',
				done: false,
			},
			{
				title: 'Eat Dinner',
				done: false,
			}
		]
	}),
	methods: {
		deleteTask(index) {
			this.$q.dialog({
				title: 'Confirm',
				message: 'Are you sure you want to delete this item?',
				cancel: true,
				persistent: true
			}).onOk(() => {
				this.tasks.splice(index, 1)
				this.$q.notify('Item has been deleted.')
			})
		},
		addTask() {
			this.tasks.push({
				title: this.newTask,
				done: false
			})
			this.newTask = ``
		}
	}
}
</script>

<style lang="scss">
.done {
	.q-item__label {
		text-decoration: line-through;
		color: #bbb;
	}
}
.no-tasks {
	opacity: .5;
}
</style>