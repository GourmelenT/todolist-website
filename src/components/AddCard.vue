<script>
export default {
  data() {
    return {
      newTask: {
        title: '',
        description: '',
        start: '',
        end: '',
        status: 'À faire',
        priority: 'Moyenne',
        completed: false,
        statusSelectToggled: false
      },
      priorities: {
        BASSE: 'Basse',
        MOYENNE: 'Moyenne',
        HAUTE: 'Haute'
      },
      status: {
        A_FAIRE: 'A faire',
        EN_COURS: 'En cours',
        TERMINEE: 'Terminée'
      }
    }
  },
  methods: {
    verifNewTask() {
      if (this.newTask.title === '') {
        alert('Veuillez entrer un titre')
        return false
      } else {
        if (this.newTask.start === '') {
          this.newTask.start = new Date().toISOString().slice(0, 10)
        }
        if (this.newTask.end === '') {
          this.newTask.end = this.newTask.start
        } else if (this.newTask.end < this.newTask.start) {
          alert('La date de fin ne peut pas être antérieure à la date de début')
          return false
        }
        return true
      }
    },
    sendNewTask() {
      if (this.verifNewTask()) {
        this.$emit('newTask', this.newTask)
        this.newTask = {
          title: '',
          description: '',
          start: '',
          end: '',
          status: 'À faire',
          priority: 'Moyenne',
          completed: false,
          statusSelectToggled: false
        }
      }
    },
    validDateIcon(event) {
      if (event.currentTarget.value === '') {
        event.currentTarget.classList.remove('valid-date-icon')
      } else {
        event.currentTarget.classList.add('valid-date-icon')
      }
    }
  }
}
</script>

<template>
  <div class="add-todo-card">
    <form>
      <div class="add-todo-card-row">
        <div class="add-todo-card-priorities-container value-container">
          <input
            v-model="newTask.priority"
            :value="priorities.BASSE"
            type="radio"
            class="add-todo-card-priority"
            id="radio-basse"
            name="priorities-radios"
          />
          <label for="radio-basse" title="Priorité basse"></label>
          <input
            v-model="newTask.priority"
            :value="priorities.MOYENNE"
            type="radio"
            class="add-todo-card-priority"
            id="radio-moyenne"
            name="priorities-radios"
          />
          <label for="radio-moyenne" title="Priorité moyenne"></label>
          <input
            v-model="newTask.priority"
            :value="priorities.HAUTE"
            type="radio"
            class="add-todo-card-priority"
            id="radio-haute"
            name="priorities-radios"
          />
          <label for="radio-haute" title="Priorité haute"></label>
        </div>

        <div class="add-todo-card-title-container value-container">
          <input
            type="text"
            class="add-todo-card-title"
            v-model="newTask.title"
            placeholder="Titre"
          />
        </div>

        <div class="add-todo-card-description-container value-container">
          <textarea
            class="add-todo-card-description"
            placeholder="Description"
            v-model="newTask.description"
          ></textarea>
        </div>

        <div class="add-todo-card-date-container value-container">
          <input
            type="date"
            class="add-todo-card-start"
            v-model="newTask.start"
            v-on:change="validDateIcon"
            title="Date de début"
          />
          <span v-if="newTask.start" class="selected-date" style="cursor: pointer">{{
            newTask.start
          }}</span>

          <input
            type="date"
            class="add-todo-card-end"
            v-model="newTask.end"
            v-on:change="validDateIcon"
            title="Date de fin"
          />
          <span v-if="newTask.end" class="selected-date">{{ newTask.end }}</span>
        </div>

        <div class="add-card">
          <button type="button" class="add-todo-card-button value-container" @click="sendNewTask">
            Ajouter
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<style scoped>
*,
*:before,
*:after {
  margin: auto;
  padding: 0;
  outline: 0;
  box-sizing: border-box;
}

.add-todo-card {
  background-color: white;
  width: 100%;
  height: 50%;
  padding: 0.8rem 1.5rem;
  margin: .5rem;
  border-radius: 0.5rem;
}

.add-todo-card-row {
  display: grid;
  grid-template-columns: 80px repeat(2, 1fr);
  grid-template-rows: 1fr 1fr;
}

.value-container {
  display: flex;
  width: fit-content;
  margin: 0;
  margin-right: 10px;
}

.add-todo-card-priority {
  appearance: none;
  display: flex;
}

label {
  display: inline-block;
  width: 17px;
  height: 17px;
  border-radius: 50%;
  transition: all 0.2s ease-in-out;
  margin-right: 5px;
  opacity: 0.3;
}

label:hover {
  transform: scale(1.1);
}

#radio-basse + label {
  background: #3fd4f4;
}

#radio-moyenne + label {
  background: #fac608;
}

#radio-haute + label {
  background: #ff6385;
}

.add-todo-card-priority:checked + label {
  opacity: 1;
  scale: 1.1;
}

.add-todo-card-title {
  border: none;
  outline: none;
  border-radius: 0.4rem;
  grid-column: span 2;
}

.add-todo-card-title:focus {
  color: white;
  background-color: #383838;
  padding: 5px 0 5px 5px;
}

.add-todo-card-start,
.add-todo-card-end {
  border: none;
  outline: none;
  margin-right: 10px;
  color: #acabab;
  width: 20px;
  cursor: pointer;
}

.selected-date {
  color: #393737;
  font-size: 11px;
  padding: 0 25px 0 0;
  cursor: pointer;
}

.add-todo-card-description-container {
  grid-column: span 3;
  width: 100%;
}

.add-todo-card-description {
  width: 100%;
  margin-bottom: 10px;
}

textarea {
  font-size: 14px;
  border: 1px none;
  border-radius: 4px;
  resize: none;
}

textarea:focus {
  background-color: #383838;
  color: white;
  padding: 5px 0 0 5px;
}

.add-todo-card-start::-webkit-calendar-picker-indicator,
.add-todo-card-end::-webkit-calendar-picker-indicator {
  background: url(file:../../../assets/date_picker_invalid.svg) no-repeat;
  background-size: 90%;
}

.valid-date-icon::-webkit-calendar-picker-indicator {
  background: url(file:../../../assets/date_picker_valid.svg) no-repeat;
  background-size: 90%;
}

.add-todo-card-date-container {
  grid-column: span 2;
  cursor: pointer;
}

button {
  /* Styles spécifiques au bouton */
  background-color: #eceaea;
  color: #333;
  padding: 5px 15px;
  font-size: 16px;
  border: none;
  border-radius: .3rem;
  cursor: pointer;
  box-shadow: 7px 7px 10px -7px rgba(0, 0, 0, 0.38);
  -webkit-box-shadow: 7px 7px 10px -7px rgba(0, 0, 0, 0.38);
  -moz-box-shadow: 7px 7px 10px -7px rgba(0, 0, 0, 0.38);
  transition: all 300ms ease-in-out;
}

button:hover {
  background-color: #acabab;
  color: #fff;
  box-shadow: none;
}

.add-card {
  display: flex;
  justify-content: end;
}

@media (max-width: 620px){
  .add-card{
    margin-top: 10px;
    grid-column: span 2;
  }


}
</style>
