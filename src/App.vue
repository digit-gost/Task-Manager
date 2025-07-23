<script>
  export default {
    name: 'App',
    data(){
      return{
        tasks:[
          {titre: "Maquette Figma", responsable: "Amina", statut: "terminé", urgent: true},
          {titre: "Connexion API", responsable: "Alioune", statut: "en cours", urgent: false},
          {titre: "Création base de données", responsable: "Fatou", statut: "à faire", urgent: true},
          {titre: "Design landing page", responsable: "Amina", statut: "en cours", urgent: false}
        ],

        selectedStatut: "",
        selectedResponsable: ""
      };
    },

    computed:{
      responsables(){
        return [... new Set(this.tasks.map(t => t.responsable))];
      },

      statuts(){
        return [... new Set(this.tasks.map(t => t.statut))];
      },

      filteredTasks(){
        return this.tasks.filter(task =>{
          const statutMatch = this.selectedStatut ? task.statut === this.selectedStatut : true;
          const responsableMatch = this.selectedResponsable ? task.responsable === this.selectedResponsable : true;
          return statutMatch && responsableMatch;
        });
      }
    }
  };
</script>

<template>
  <div class="container">
    <h1>Task Manager</h1>
    <div class="filters">
      <label>
        Statut :
        <select v-model="selectedStatut">
          <option value="">Tous</option>
          <option v-for="statut in statuts" :key="statut" :value="statut">{{ statut }}</option>
        </select>
      </label>

      <label for="">
        Responsable :
        <select v-model="selectedResponsable">
          <option value="">Tous</option>
          <option v-for="resp in responsables" :key="resp" :value="resp"> {{ resp }}</option>
        </select>
      </label>
    </div>

    <div class="count">
      <strong>{{ filteredTasks.length }}</strong> tâche(s) visible(s)
    </div>

    <ul class="task-list">
      <li v-for="task in filteredTasks" :key="task.titre" :class="['task', task.statut, {urgent: task.urgent}]">
        <span v-if="task.urgent" class="urgent-icon">🆘</span>
        <span class ="titre"> {{ task.titre }}</span> -
        <span class="responsable">{{ task.responsable }}</span> -
        <span class="statut"> {{ task.statut }}</span>
      </li>
    </ul>

    <div v-if="filteredTasks.length === 0" class="no-task">
      Aucune Tâche Trouvée
    </div>
  </div>
</template>

<style scoped>
/* From Univers.io */
  .container{
    max-width: 700px;
    margin: 40px auto;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #f8fafc, 60%, #e0e7ff 100%);
    border-radius: 18px;
    box-shadow: 0 8px 32px rgba(60, 60, 120, 0.12);
    padding: 2em 2.5em;
  }

  h1{
    text-align: center;
    color: blue;
    font-size: 2.5em;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 1.5em;
  }

  .filters{
    display: flex;
    gap: 2em;
    margin-bottom: 2em;
    justify-content: center;
  }

  label{
    font-weight: bold;
    color: mediumslateblue;
  }

  select{
    margin-left: 0.5em;
    padding: 0.4em 1em;
    border-radius: 9px;
    border: solid 1px #ccc;
    background: #eef2ff;
    color: #3730a3;
    font-size: 1em;
    transition: border 0.3s;
  }

  select:focus{
    border: 1.5px solid #6366f1;
    outline: none;
  }

  .count{
    margin-bottom: 1.5em;
    text-align: center;
    font-size: 1.1em;
    color: #3730a3;
  }

  .task-list{
    list-style: none;
    padding: 0;
  }

  .task{
    padding: 1em 1.2em;
    margin-bottom: 1em;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(60, 60, 120, 0.1);
    display: flex;
    align-items: center;
    gap: 1em;
    font-size: 1.08em;
    background: white;
    position: relative;
    transition: transform 0.2s;
  }

  .task:hover{
    transform: scale(1.03);
    box-shadow: 0 4px 16px rgba(60, 60, 120, 0.15);
  }

  .task.urgent{
    border-left: 6px solid red;
    background: linear-gradient(90deg, #fee2e2 80%, #fff 100%);  
  }

  .task.à\ faire{
    background: linear-gradient(90deg, #e0e7ef 80%, #fff 100%);
    color: grey;
  }

  .task.en\ cours{
    background: linear-gradient(90deg, #fef9c3 80%, #fff 100%);
    color: orange;  
  }

  .task.terminé{
    background: linear-gradient(90deg, #d1fae5 80%, #fff 100%);
    color: green;  
  }

  @keyframes blink{
    0%, 100% { opacity: 1; }
    50% { opacity: 0.5; }
  }

  .titre{
    font-weight: bold;
    color: #3730a3;
  }

  .responsable{
    font-style: italic;
    color: #4a5568;
  }

  .statut{
    display: flex;
    align-items: center;
    gap: 0.5em;
  }

  .statut::before{
    content: '';
    display: inline-block; 
    width: 1em;
    height: 1em;
    border-radius: 50%;
    margin-right: 0.2em;
  }

  .task.à\ faire .statut::before {
    background: grey;
  }

  .task.en\ cours .statut::before {
    background: orange;
  }

  .task.terminé .statut::before {
    background: green;
  }

  .no-task{
    margin-top: 2em;
    color: #6366f1;
    text-align: center;
    font-size: 1.2em;
    font-weight: bolder;
    letter-spacing: 1px;
  }
</style>
