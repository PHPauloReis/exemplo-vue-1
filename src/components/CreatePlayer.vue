<template>
  <v-card class="mx-auto" max-width="960">
    <v-layout>
      <v-app-bar color="primary" density="compact">
        <v-app-bar-title>Cadastre o seu time</v-app-bar-title>
      </v-app-bar>

      <v-main>
        <v-container fluid>
          
          <v-form ref="form" v-model="valid" @submit.prevent>
            <v-container>
              <v-row>
                <v-col
                  cols="12"
                  md="6"
                >
                  <v-text-field
                    v-model="player.name"
                    :counter="10"
                    :rules="nameRules"
                    label="Nome do jogador"
                    required
                  ></v-text-field>
                </v-col>

                <v-col
                  cols="12"
                  md="6"
                >
                  <v-select
                    v-model="player.team"
                    :items="teams"
                    label="Escolha um time"
                  ></v-select>
                  <!-- <v-text-field
                    v-model="player.team"
                    :counter="10"
                    :rules="teamRules"
                    label="Time do jogador"
                    required
                  ></v-text-field> -->
                </v-col>

                <v-col
                  cols="12"
                  md="6"
                >
                  <v-text-field
                    v-model="player.cpf"
                    :rules="cpfRules"
                    label="CPF"
                    required
                  ></v-text-field>
                </v-col>

                <v-col
                  cols="12"
                >
                  <v-btn @click="savePlayer" class="mt-2" type="submit" color="success" block>Cadastrar jogador</v-btn>
                </v-col>

              </v-row>
            </v-container>
          </v-form>
          
        </v-container>
      </v-main>
    </v-layout>
  </v-card>

  <v-card class="mx-auto mt-5" max-width="960">
    <v-layout>
      <v-app-bar color="purple-darken-4" density="compact">
        <v-app-bar-title>Jogadores cadastrados</v-app-bar-title>
      </v-app-bar>

      <v-main>
        <v-container fluid>
          
          <v-data-table-virtual
            :headers="headers"
            :items="players"
            item-value="name"
          ></v-data-table-virtual>
          
        </v-container>
      </v-main>
    </v-layout>
  </v-card>

  <v-dialog
    v-model="dialog"
    width="auto"
  >
    <v-card
      max-width="400"
      prepend-icon="mdi-check-bold"
      :text="dialogMessage"
      :title="dialogTitle"
    >
      <template v-slot:actions>
        <v-btn
          class="ms-auto"
          text="Ok"
          @click="dialog = false"
        ></v-btn>
      </template>
    </v-card>
  </v-dialog>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      dialogTitle: '',
      dialogMessage: '',
      valid: false,
      player: { name: '', team: '', cpf: '' },
      players: [],
      nameRules: [
        value => {
          if (value && value.trim()) return true

          return 'Informe o nome do jogador.'
        },
        value => {
          if (value && value.length <= 10) return true

          return 'Nome do jogador muito grande.'
        },
      ],
      teamRules: [
        value => {
          if (value && value.trim()) return true

          return 'Informe o nome do time.'
        },
        value => {
          if (value && value.length <= 10) return true

          return 'Nome do time muito grande.'
        },
      ],
      cpfRules: [
        value => {
          if (value && value.trim()) return true

          return 'Você esqueceu de informar o CPF.'
        },
        value => {
          const strCPF = value ?? ""
          let isValid = true

          var sum;
          var rest;
          sum = 0;
          if (strCPF == "00000000000") isValid = false;

          for (let i=1; i<=9; i++) sum = sum + parseInt(strCPF.substring(i-1, i)) * (11 - i);
          rest = (sum * 10) % 11;

          if ((rest == 10) || (rest == 11))  rest = 0;
          if (rest != parseInt(strCPF.substring(9, 10)) ) isValid = false;

          sum = 0;
          for (let i = 1; i <= 10; i++) sum = sum + parseInt(strCPF.substring(i-1, i)) * (12 - i);
          rest = (sum * 10) % 11;

          if ((rest == 10) || (rest == 11))  rest = 0;
          if (rest != parseInt(strCPF.substring(10, 11) ) ) isValid = false;

          if (isValid) {
            return true;
          }

          return 'O CPF informado não parece ser válido!'
        }
      ],
      headers: [
        { title: 'Jogador', align: 'start', key: 'name' },
        { title: 'Time', align: 'start', key: 'team' },
        { title: 'CPF', align: 'start', key: 'cpf' },
      ],
      teams: ['UNIDEV', 'CORINTIA', 'FRAMENGO', 'BAIA', 'MARCELONA']
    }),
    methods: {
      savePlayer() {
        if (this.valid) {

          const totalOfPlayersInTeam = this.countTeams(this.player.team)

          if (totalOfPlayersInTeam > 2) {
            this.dialogTitle = "Oups!!!"
            this.dialogMessage = "Esse time não aceita mais jogadores!"
            this.dialog = true
            return
          }

          this.players.push({ ...this.player })
          this.$refs.form.reset()
          this.dialogTitle = "Uêba!!!"
          this.dialogMessage = "Jogador cadastrado com sucesso!"
          this.dialog = true          
        }
      },
      countTeams(team) {
        let total = 0;

        this.players.map((player) => {
          if (player.team === team) {
            total++
          }
        })

        return total
      }
    }
  }
</script>
