<template>
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <form>
          <div class="form-group">
            <label for="feature">Feature:</label>
            <input type="text" class="form-control" id="feature" v-model="feature" required>
          </div>
          <div class="form-group" v-for="(scenario, scenarioIndex) in scenarios" :key="scenarioIndex">
            <label for="scenario">Scenario {{ scenarioIndex + 1 }}:</label>
            <div class="row" v-for="(step, stepIndex) in scenario.steps" :key="stepIndex">
              <div class="col-md-4">
                <select class="form-control" v-model="step.keyword" required>
                  <option value="Given">Given</option>
                  <option value="When">When</option>
                  <option value="Then">Then</option>
                  <option value="And">And</option>
                  <option value="But">But</option>
                </select>
              </div>
              <div class="col-md-8">
                <input type="text" class="form-control" v-model="step.text" required>
              </div>
              <div class="col-md-2">
                <button type="button" class="btn btn-danger" @click="deleteStep(scenarioIndex, stepIndex)">Supprimer</button>
              </div>
            </div>
            <button type="button" class="btn btn-primary" @click="addStep(scenarioIndex)">Ajouter une étape</button>
            <button type="button" class="btn btn-danger" @click="deleteScenario(scenarioIndex)">Supprimer le scénario</button>
          </div>
          <button type="button" class="btn btn-primary" @click="addScenario">Ajouter un scénario</button>
          <button type="button" class="btn btn-success" @click="createSpecflow">Créer le fichier Specflow</button>
        </form>
      </div>
      <div class="col-md-6">
        <h3>{{ feature }}</h3>
        <hr>
        <div v-for="(scenario, scenarioIndex) in scenarios" :key="scenarioIndex">
          <h4>{{ scenario.keyword }} {{ scenario.text }}</h4>
          <div v-for="(step, stepIndex) in scenario.steps" :key="stepIndex">
            <p>{{ step.keyword }} {{ step.text }}</p>
          </div>
          <button type="button" class="btn btn-danger" @click="deleteScenario(scenarioIndex)">Supprimer le scénario</button>
          <hr>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      feature: '',
      scenarios: [{ keyword: 'Scenario', text: '', steps: [{ keyword: 'Given', text: '' }] }]
    }
  },
  methods: {
    addScenario() {
      this.scenarios.push({ keyword: 'Scenario', text: '', steps: [{ keyword: 'Given', text: '' }] });
    },
    addStep(scenarioIndex) {
      this.scenarios[scenarioIndex].steps.push({ keyword: 'Given', text: '' });
    },
    deleteScenario(scenarioIndex) {
      this.scenarios.splice(scenarioIndex, 1);
    },
    deleteStep(scenarioIndex, stepIndex) {
      this.scenarios[scenarioIndex].steps.splice(stepIndex, 1);
    },
    createSpecflow() {
      let specflow = `#language: en\nFeature: ${this.feature}\n\n`;
      this.scenarios.forEach(scenario => {
        specflow += `${scenario.keyword}: ${scenario.text}\n`;
        scenario.steps.forEach(step => {
          specflow += `${step.keyword} ${step.text}\n`;
        });
        specflow += '\n';
      });
      
      const filename = `${this.feature}.feature`;
      const blob = new Blob([specflow], { type: 'text/plain;charset=utf-8' });
      if (navigator.msSaveBlob) {
        navigator.msSaveBlob(blob, filename);
      } else {
        const link = document.createElement('a');
        if (link.download !== undefined) {
          const url = URL.createObjectURL(blob);
          link.setAttribute('href', url);
          link.setAttribute('download', filename);
          link.style.visibility = 'hidden';
          document.body.appendChild(link);
          link.click();
          document.body.removeChild(link);
        }
      }
    }
  }
}
</script>
