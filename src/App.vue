<script setup>
    import HeaderComponent from './components/HeaderComponent.vue'
    import TaskList from './components/TaskList.vue'
</script>

<template>
    <header>
        <div class="wrapper">
            <HeaderComponent @newTask="addTask"/>
        </div>
    </header>

    <main>
        <TaskList
            :tasks="tasks"
            :deleteTask="deleteTask"
            :updateTask="updateTask"
            :toggleStatusSelect="toggleStatusSelect"
            :getTaskedClass="getTaskedClass"
            :toggleActive="toggleActive"
            :filterTasks="filterTasks"/>
    </main>
</template>

<script>
    export default {
        mounted() {
            this.loadTasksFromLocalStorage();
        },

        data() {
            return {
                tasks: [
                    {
                        title: 'Faire les courses',
                        description: 'Acheter du pain',
                        start: '2023-06-13',
                        end: '2023-06-14',
                        status: 'À faire',
                        priority: '',
                        completed: false,
                        statusSelectToggled: false
                    }, {
                        title: "Nuits d'étoiles filantes",
                        description: 'Delta Aquariids',
                        start: '2023-07-30',
                        end: '2023-07-31',
                        status: 'À faire',
                        priority: 'Moyenne',
                        completed: false,
                        statusSelectToggled: false
                    }
                ],
                filters: []
            }
        },
        methods : {
            saveTasksToLocalStorage() {
                localStorage.setItem('tasks', JSON.stringify(this.tasks));
            },

            loadTasksFromLocalStorage() {
                const savedTasks = localStorage.getItem('tasks');
                if (savedTasks) {
                    this.tasks = JSON.parse(savedTasks);
                }
            },

            addTask(newTask) {
                this
                    .tasks
                    .push({
                        ...newTask
                    });
                this.saveTasksToLocalStorage(); // Sauvegarder les tâches dans le localStorage après l'ajout
            },

            updateTask(index) {
                this.tasks[index].completed = !this.tasks[index].completed;
                this.tasks[index].status = this.tasks[index].completed
                    ? 'Terminé'
                    : 'À faire';
                this.tasks[index].statusSelectToggled = false;
                this.saveTasksToLocalStorage();
            },

            toggleStatusSelect(index, value) {
                if (!this.tasks[index].completed || this.tasks[index].statusSelectToggled) {
                    this.tasks[index].statusSelectToggled = !this.tasks[index].statusSelectToggled;
                    if (value) {
                        this.tasks[index].status = value;
                    }
                }
            },
            
            deleteTask(index) {
                this
                    .tasks
                    .splice(index, 1)
                this.saveTasksToLocalStorage();
            },

            getTaskedClass(priority) {
                switch (priority) {
                    case 'Basse':
                        return 'tasked-priority-low'
                    case 'Moyenne':
                        return 'tasked-priority-medium'
                    case 'Haute':
                        return 'tasked-priority-high'
                    default:
                        return 'tasked-priority-none' // Aucune classe par défaut si la priorité n'est pas définie
                }
            },

            toggleActive(event) {
                const li = event.target;
                if (li.tagName === 'LI') {
                    li
                        .classList
                        .toggle('active');
                    this.addOrRemoveFilter(li.classList[0]);
                }
            },

            addOrRemoveFilter(value) {
                var index = this
                    .filters
                    .indexOf(value)
                if (index === -1) {
                    this
                        .filters
                        .push(value)
                } else {
                    this
                        .filters
                        .splice(index, 1)
                }
            },

            filterTasks() {
                this
                    .tasks
                    .sort((a, b) => {
                        for (let i = 0; i < this.filters.length; i++) {
                            let fct;
                            switch (this.filters[i]) {
                                case 'flt-etat':
                                    fct = compareEtat;
                                    break;
                                case 'flt-priorities':
                                    fct = comparePriorite;
                                    break;
                                case 'flt-start':
                                    fct = compareStart;
                                    break;
                                case 'flt-end':
                                    fct = compareEnd;
                                    break;
                            }
                            const result = fct(a, b);
                            if (result !== 0) {
                                return result;
                            }
                        }
                    });
            }
        }
    }

    const compareEtat = (a, b) => {
        const etats = ['À faire', 'En cours', 'Terminé'];
        const indexA = etats.indexOf(a.status);
        const indexB = etats.indexOf(b.status);
        if (indexA > indexB) {
            return 1;
        } else if (indexA < indexB) {
            return -1;
        } else {
            return 0;
        }
    }

    const comparePriorite = (a, b) => {
        const priorites = ['Haute', 'Moyenne', 'Basse', ''];
        const indexA = priorites.indexOf(a.priority);
        const indexB = priorites.indexOf(b.priority);
        if (indexA > indexB) {
            return 1;
        } else if (indexA < indexB) {
            return -1;
        } else {
            return 0;
        }
    }

    const compareStart = (a, b) => {
        const dateA = new Date(a.start);
        const dateB = new Date(b.start);
        if (dateA > dateB) {
            return 1;
        } else if (dateA < dateB) {
            return -1;
        } else {
            return 0;
        }
    }

    const compareEnd = (a, b) => {
        const dateA = new Date(a.end);
        const dateB = new Date(b.end);
        if (dateA > dateB) {
            return 1;
        } else if (dateA < dateB) {
            return -1;
        } else {
            return 0;
        }
    }
</script>

<style scoped>
    header {
        line-height: 1.5;
    }

    .logo {
        display: block;
        margin: 0 auto 2rem;
    }
    @media (min-width: 1024px) {
        header {
            display: flex;
            place-items: center;
            padding-right: calc(var(--section-gap) / 2);
        }

        .logo {
            margin: 0 2rem 0 0;
        }

        header .wrapper {
            display: flex;
            place-items: flex-start;
            flex-wrap: wrap;
        }
    }

</style>