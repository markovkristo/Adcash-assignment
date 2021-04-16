<template>
  <div>
    <p>
      <strong>Enter tags here</strong> (commas, semicolons and new lines allowed)
    </p>
    <div class="container">
      <div>
        <textarea id="textArea" v-model="text"></textarea>
      </div>
      <div class="tagArea">
        <span v-for="(num, index) in numbers" v-bind:key="num">
          <button v-if="num < 0" @click="removeTag(index)" id="negativeNr"> X </button>
          <button v-else @click="removeTag(index)" id="positiveNr"> X </button>
        </span>
      </div>
    </div>
    <div class="buttons">
      <button id="editButton" @click="editData"> Edit </button>
      <button id="addButton" @click="saveText(text)"> Add </button>
    </div>
    <div class="edit"  v-if="editing">
      <form class="editForm">
        <h2>Your inputs: </h2>
        <textarea name="msg" id="editArea" v-model="editedInput"></textarea>
        <button id="saveButton" @click="saveChanges"> Save </button>
      </form>
    </div>
  </div>
</template>

<script>

export default {
  name: "TextArea",
  data() {
    return {
      text: null,
      editedInput: null,
      numbers: [],
      editing: false,
    };
  },
  methods: {
    /**
     * Method to save the Users input and format it.
     */
    saveText: function (text) {
      console.warn(text);
      this.inputFormatting(text);
      this.text = null;
    },

    /**
     * Method to save input to the numbers array and to localStorage.
     * @param input, Users input to the text area.
     */
    inputFormatting: function (input) {
      if (input !== null) {
        let stringList = input.split(/\s*[,\n;]+\s*/); //Split the input from comma, semicolon or new line.
        this.constructNumberList(stringList);
        localStorage.setItem("inputs", this.numbers); // Update local storage.
      }
    },

    /**
     * Method to sort out non-numeric numbers.
     * @param numbers, Input after splitting it from comma, semicolon and new line.
     */
    constructNumberList: function (numbers) {
      for (let i = 0; i < numbers.length; i++) {
        let number = numbers[i];
        if (!isNaN(number) && number !== "") // Check-s if the string is number or something else.
          this.numbers.push(number);
      }
    },

    /**
     * Method to remove the right tags on click from the tag area.
     * @param index, Shows which element we must remove from the numbers list.
     */
    removeTag: function (index) {
      this.$delete(this.numbers, index);
      localStorage.setItem("inputs", this.numbers); // Update local storage
    },


    /**
     * Method to open new text area where you change your data.
     */
    editData: function () {
      this.editedInput = this.numbers;
      this.editing = true; // will show the editing panel.
    },
    saveChanges: function () {
      this.editing = false;
      let arr = this.editedInput.split(/\s*[,\n;]+\s*/) //Split the data which we want to edit from comma, semicolon or new line.
      this.numbers = arr.filter(function (el) { // Will filter out elements which aren't numbers.
        if (el === '') //if el is empty string then don't return nothing
          return null;
        return !isNaN(el); //if el isn't a number don't return it.
      });
      localStorage.setItem("inputs", this.numbers); // Update local storage.
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import "../assets/variables.scss";


.container {
  border-radius: $borderRadius;
  border: $borderWidth $borderColor;
  width: $width;

  /* Input area style */
  #textArea{
    height: $height - 100;
    width: $width - 15;
    border-style: $textAreaBorder;
    border-bottom-color: $bottomBorderColor;
    border-width: $borderWidth;
    margin: $textAreaMargin;
    resize: none;
    outline: none;
  }

  /* Tag area style*/
  .tagArea {
    height: $height - 200;
    overflow: auto;
    Button{
      margin: $tagMargin;
      padding: $tagPadding;
      border-radius: $borderRadius;
      cursor: $cursorPointer;
      border: none;
      font-weight: $black-weight;
      font-size: $fontSize;
      outline: none;
    }

    #negativeNr {
      background: $negativeNrColor;
    }

    #positiveNr {
      background: $positiveNrColor;
    }
  }
}



/* Buttons styles */
#editButton, #addButton, #saveButton {
  border-radius: $borderRadius;
  font-weight: $buttonFontWeight;
  color: $white;
  padding: $buttonPadding;
  background-color: $blue;
  margin: $buttonMargin;
  cursor: $cursorPointer;
  font-size: inherit;
}

#editButton {
  float: left;
}

#addButton {
  float: right;
}


/* The editPanel style - hidden by default */
.edit{
  margin-top: $editMarginTop;
  display: flex;
  border: $borderWidth $borderColor;
  border-radius: $borderRadius;
  .editForm {
    padding: $editFormPadding;
    background-color: $white;
    #editArea {
      border-radius: $borderRadius;
      border: $borderWidth $borderColor;
      padding: $editFormPadding + 5;
      height: $height - 100;
      width: $width - 375;
      resize: none;
      outline: none;
    }
  }
}

</style>