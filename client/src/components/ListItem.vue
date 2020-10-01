<template>
	<li class="list-item" :class="{ done: isDone }">
        <input type="checkbox" class="checkbox" :id="idComputed" @change="onTaskStatusChange" :checked="isDone">
        <label class="mr-3" :for="idComputed"></label>
        <span class="text">
            {{ text }}
        </span>
        <span class="icon-delete" @click="onTaskDelete"></span>
    </li>
</template>

<script>
	export default {
        name: "ListItem",
        props: {
            id: {
                type: Number,
                default: 0
            },
            text: {
                type: String,
                default: ""
            },
            isDone: {
                type: Boolean,
                default: false
            }
        },
        computed: {
            idComputed() {
                return `item-${this.id}`
            }
        },
        methods: {
            /**
             * Event: on task status changed
             */
            onTaskStatusChange(e) {
                const checked = e.target.checked
                this.$emit("eventTaskStatusChange", this.id, checked)

                if (checked) {
                    console.log("done : " + this.text)
                    window._paq.push(['trackEvent', 'Todo', 'Done', 'doneTodo', this.text]);
                } else {
                    console.log("cancel : " + this.text)
                    window._paq.push(['trackEvent', 'Todo', 'Cancel', 'cancelTodo', this.text]);
                }
            },

            /**
             * Event: on task deleted
             */
            onTaskDelete(e) {
                this.$emit("eventTaskDelete", this.id)
                console.log("delete : " + this.text)
                window._paq.push(['trackEvent', 'Todo', 'Delete', 'deleteTodo', this.text]);
            }
        }
    }
</script>

