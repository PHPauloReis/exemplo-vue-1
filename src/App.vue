<template>

  <div class="container">
    <div class="row">
      <div class="col-md-12">

        <div class="card mt-5">
          <div class="card-header">Cadastro de alunos</div>
          <div class="card-body">
            
            <div class="row">
              <div class="col-md-6 mb-3">
                <label for="name" class="form-label">Nome do aluno</label>
                <input type="text" class="form-control" id="name" v-model="student.name">
              </div>
              <div class="col-md-6 mb-3">
                <label for="age" class="form-label">Idade do aluno</label>
                <input type="number" class="form-control" id="age" v-model="student.age">
              </div>
              <div class="col-md-6 mb-3">
                <label for="cpf" class="form-label">CPF do aluno</label>
                <input type="text" class="form-control" id="cpf" v-model="student.cpf">
              </div>
              <div class="col-md-12 mb-3">
                <button class="btn btn-success" @click="createNewStudent">Cadastrar</button>
              </div>
            </div>

          </div>
        </div>

        <div class="card mt-5">
          <div class="card-header">Alunos cadastrados</div>
          <div class="card-body">

            <table class="table table-striped">
              <thead>
                <tr>
                  <td>Nome</td>
                  <td>CPF</td>
                  <td>Idade</td>
                </tr>
              </thead>
              <tbody>
                <tr v-for="student of students" :key="student.cpf">
                  <td>{{ student.name }}</td>
                  <td>{{ student.cpf }}</td>
                  <td>{{ student.age }}</td>
                </tr>
              </tbody>
            </table>

          </div>
        </div>

      </div>
    </div>
  </div>
  
</template>

<script>

export default {
  data() {
    return {
      student: { name: "", age: "", cpf: "" },
      students: [
        { name: "Tífani", age: "18", cpf: "123.123.123-99" },
        { name: "Isabela", age: "20", cpf: "123.123.444-99" }
      ]
    }
  },
  methods: {
    clearWhiteSpacesInStudent() {
      this.student.name.trim()
      this.student.cpf.trim()
    },

    isStringEmpty(string) {
      return string.length === 0
    },

    isStringLessThen(string, length) {
      return string.length < length
    },

    createNewStudent() {
      this.clearWhiteSpacesInStudent()

      if (
        this.isStringEmpty(this.student.name) ||
        this.isStringEmpty(this.student.age) ||
        this.isStringEmpty(this.student.cpf)
      ) {
        alert("Todos os dados do aluno são obrigatórios!")
        return
      }

      if (this.isStringLessThen(this.student.name, 3)) {
        alert("O nome do aluno deve ter ao menos 3 letras!")
        return
      }

      if (this.isStringLessThen(this.student.cpf, 14)) {
        alert("O CPF informado parece não ser válido!")
        return
      }

      if (this.student.age < 1) {
        alert("O aluno deve ter ao menos 1 ano")
        return
      }

      this.students.push(this.student)
    }
  }
}

</script>