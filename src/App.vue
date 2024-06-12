<template>
  <div class="container">
    <form @submit.prevent="submitForm" class="form">
      <div class="form-elements">
        <label for="name" class="form-label">Name:</label>
        <input
          type="text"
          class="form-input"
          v-model="res.name"
          @input="validateName"
          required
        />
        <div v-if="showError" style="color: red">{{ showError }}</div>
      </div>

      <div class="form-elements">
        <label for="minor" class="form-label">Minor:</label>
        <input type="checkbox" v-model="res.minor" class="form-checkbox" />
      </div>

      <div class="form-elements">
        <label for="date" class="form-label">Date:</label>
        <input type="date" class="form-input" v-model="res.date" required />
      </div>

      <div class="form-elements">
        <button type="submit" class="form-button">Submit</button>
      </div>
    </form>

    <div v-if="Result.length" class="submitted-forms">
      <h2>Submitted Forms</h2>
      <ul class="form-list">
        <li v-for="(itm, index) in Result" :key="index" class="form-list-item">
          <span class="form-item-name">{{ itm.name }}</span>
          <span class="form-item-minor">{{ itm.minor ? "Yes" : "No" }}</span>
          <span class="form-item-date">{{ itm.date }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      res: {
        name: "",
        minor: false,
        date: "",
        showError: "",
      },
      Result: [],
      Blocks: [],
    };
  },
  methods: {
    validateName() {
      const isNumber = /\d/;
      if (isNumber.test(this.res.name)) {
        this.showError = "Please enter some string.";
      } else {
        this.showError = "";
      }
    },
    submitForm() {
      if (this.res.name.trim() && !this.showError && this.res.date) {
        this.Result.push({
          name: this.res.name,
          minor: this.res.minor,
          date: this.res.date,
        });
        const nameObject = {
          token: "PERSON_NAME",
          type: "text",
          props: {
            title: this.res.name,
            required: true,
            palceHolder: "Enter name",
          },
        };

        const minorObject = {
          token: "IS_PERSON_MINOR",
          type: "checkbox",
          props: {
            title: this.res.minor,
            default: false,
          },
        };
        const dobObject = {
          token: "PERSON_DOB",
          type: "date",
          props: {
            title: this.res.date,
            default: false,
            palceHolder: "Eg: 01/01/2000",
          },
        };
        this.Blocks.push(nameObject, minorObject, dobObject);
        this.res.name = "";
        this.res.date = "";
        this.res.minor = "";
        console.log("Blocks", JSON.parse(JSON.stringify(this.Blocks)));
      } else {
        alert("Please fill required fields.");
      }
    },
  },
};
</script>

<style scoped>
form div {
  margin-bottom: 10px;
}
.form-container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.form {
  display: flex;
  flex-direction: column;
}

.form-elements {
  margin-bottom: 15px;
}

.form-label {
  margin-bottom: 5px;
  font-weight: bold;
}

.form-input {
  width: 100%;
  padding: 8px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.form-checkbox {
  margin-left: 0;
}

.form-button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}
.submitted-forms {
  margin-top: 20px;
}

.form-list {
  list-style-type: none;
  padding: 0;
}

.form-list-item {
  background-color: #e9ecef;
  border: 1px solid #ced4da;
  border-radius: 5px;
  margin-bottom: 10px;
  padding: 10px;
  display: flex;
  justify-content: space-between;
}

.form-item-name {
  font-weight: bold;
}

.form-item-minor,
.form-item-date {
  margin-left: 10px;
}
</style>
