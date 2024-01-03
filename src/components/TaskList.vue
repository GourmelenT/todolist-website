<script setup>
    import ChangeStatus from './ChangeStatus.vue';
    defineProps([
        'tasks',
        'deleteTask',
        'updateTask',
        'toggleStatusSelect',
        'getTaskedClass',
        'toggleActive',
        'filterTasks'
    ])
</script>

<template>
    <div class="filter-bar">
        <ul @click="toggleActive">
            <li class="flt-etat filter-bar-nav">Etat</li>
            <li class="flt-priorities filter-bar-nav">Priorités</li>
            <li class="flt-start filter-bar-nav">Date début</li>
            <li class="flt-end filter-bar-nav">Date de fin</li>
            <li class="filter-bar-nav">
                <button class="btn-filter" @click="filterTasks">Filtrer</button>
            </li>
        </ul>
    </div>
    <div
        v-for="(task, index) in tasks"
        :key="index"
        :class="getTaskedClass(task.priority)"
        class="tasked"
        @click="toggleDescription">
        <div class="tasked-box">
            <input
                type="checkbox"
                @change="() => this.updateTask(index)"
                :checked="task.completed"
                title="Terminer la tâche"/>
            <span title="Titre de la tâche" class="task-title">{{ task.title }}</span>
            <span title="Date de début de la tâche" class="task-start">{{ task.start }}</span>
            <span title="Date de fin de la tâche" class="task-end">{{ task.end }}</span>
            <ChangeStatus
                v-if="task.statusSelectToggled"
                class="update-status"
                :index="index"
                :updateStatus="updateStatus"/>
            <span v-else title="Statut de la tâche" class="update-status" @click="() => toggleStatusSelect(index)">{{ task.status }}</span>
            <span title="Supprimer la tâche" @click="deleteTask(index)" class="deleteTask">
                <span class="material-icons" id="deleteButton">
                    close
                </span>
            </span>
        </div>
        <span title="Description détaillée de la tâche" class="description">{{ task.description }}</span>
    </div>
</template>

<script>
    export default {
        methods : {
            updateStatus(index, value) {
                this.toggleStatusSelect(index, value);
            }
        }
    }

    const toggleDescription = (event) => {
        const tasked = event.currentTarget
        const checkbox = tasked.querySelector('input[type="checkbox"]')
        const description = tasked.querySelector('.description')

        if (event.target === checkbox || event.target.classList.contains('update-status')) {
            return // Ne fait rien si le clic est sur la checkbox ou sur le bouton de changement de statut
        }

        if (description.style.display === 'block') {
            description.style.display = 'none'
        } else {
            description.style.display = 'block'
        }
    }
</script>

<style scoped>
    .tasked {
        margin: 1rem;
        background-color: rgba(86, 84, 84, 0.197);
        border-radius: 0.7rem;
        border-left: solid 11.5px #f567;
        transition: all 200ms ease-in-out;
        cursor: pointer;
    }

    .tasked-priority-low {
        border-left: solid 11.5px #3fd4f4;
        /* Couleur de bordure pour 'Faible' */
    }

    .tasked-priority-medium {
        border-left: solid 11.5px #fac608;
        /* Couleur de bordure pour 'Moyenne' */
    }

    .tasked-priority-high {
        border-left: solid 11.5px #ff6385;
        /* Couleur de bordure pour 'Haute' */
    }
    .tasked-priority-none {
        border-left: solid 11.5px rgba(59, 58, 58, 0.7);
        /* Couleur de bordure pour 'Haute' */
    }

    .description {
        display: none;
        padding: 5px 20px;
        font-size: 12px;
        font-style: italic;
    }

    .tasked-box {
        display: grid;
        grid-template-columns: 37px auto 150px 100px 100px 40px;
        height: 45px;
        text-align: center;
        align-items: center;
    }
    .tasked:hover {
        background-color: rgb(86, 84, 84);
        transition: all 300ms ease-in-out;
    }

    .tasked-box span#deleteButton {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 5px;
        margin-right: 5px;
    }
    .tasked-box span#deleteButton:hover {
        background-color: rgba(174, 68, 68, 0.673);
        border-color: rgba(174, 68, 68, 0.673);
        color: rgb(255, 255, 255);
        border-radius: 4rem;
        cursor: pointer;
        transition: all 300ms ease-in-out;
    }

    input[type='checkbox'] {
        cursor: pointer;
    }

    ul {
        display: flex;
        align-items: center;
        justify-content: center;
        flex-wrap: wrap;
    }

    ul li {
        display: flex;
        align-items: center;
        justify-content: center;
        margin: 5px 10px;
        list-style-type: none;
        width: 90px;
        height: 30px;
        border-radius: 0.7rem;
    }

    /* ------------------------ */
    ul li:nth-child(1) {
        border: solid 1px rgb(209, 162, 162);
    }
    ul li:nth-child(1):hover {
        cursor: pointer;
        background-color: rgba(209, 162, 162);
    }
    ul li:nth-child(1).active {
        background-color: rgba(209, 162, 162, 0.72);
        color: white;
    }

    /* ------------------------ */
    ul li:nth-child(2) {
        border: solid 1px rgb(194, 149, 35);
    }
    ul li:nth-child(2):hover {
        background-color: rgb(194, 149, 35);
        cursor: pointer;
    }
    ul li:nth-child(2).active {
        background-color: rgb(194, 149, 35, 0.72);
        color: white;
    }

    /* ------------------------ */
    ul li:nth-child(3) {
        border: solid 1px rgb(79, 145, 36);
    }
    ul li:nth-child(3):hover {
        cursor: pointer;
        background-color: rgb(79, 145, 36);
    }
    ul li:nth-child(3).active {
        background-color: rgb(79, 145, 36, 0.72);
        color: white;
    }

    /* ------------------------ */
    ul li:nth-child(4) {
        border: solid 1px rgb(170, 44, 44);
    }
    ul li:nth-child(4):hover {
        cursor: pointer;
        background-color: rgb(170, 44, 44);
    }
    ul li:nth-child(4).active {
        background-color: rgb(170, 44, 44, 0.72);
        color: white;
    }

    /* ------------------------ */

    button {
        /* Styles spécifiques au bouton */
        background-color: #eceaea;
        color: #333;
        padding: 2px 15px;
        font-size: 16px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        box-shadow: 7px 7px 10px -7px rgba(255, 255, 255, 0.38);
        -webkit-box-shadow: 7px 7px 10px -7px rgba(255, 255, 255, 0.38);
        -moz-box-shadow: 7px 7px 10px -7px rgba(255, 255, 255, 0.38);
        transition: all 300ms ease-in-out;
    }
    button:hover {
        background-color: #acabab;
        color: #fff;
        box-shadow: none;
    }
    @media (max-width: 1030px) {
        .filter-bar {
            margin-top: 2rem;
        }
    }
    @media (max-width: 620px) {
        .tasked-box {
            grid-template-columns: 37px auto repeat(2, 150px);
            grid-template-rows: auto auto;
        }

        .update-status {
            grid-column: span 2;
            text-align: start;
        }

        input[type='checkbox'] {
            grid-row: span 3;
        }

        .tasked-box span#deleteButton {
            margin-right: 61px;
        }
        .tasked-box span#deleteButton:hover {
            color: rgb(129, 66, 66);
        }

        .task-title {
            grid-column: span 3;
            text-align: start;
            font-weight: 700;
            font-size: 16px;
        }

        .task-start {
            grid-column: span 2;
            text-align: start;
        }
        .task-end {
            text-align: start;
        }

        .tasked-box {
            height: 85px;
        }
    }

</style>