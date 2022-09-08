<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "",
      animalShelters: [],
      currentAnimalShelter: {},
    };
  },

  created: function () {
    this.indexAnimalShelters();
  },
  methods: {
    indexAnimalShelters: function () {
      axios.get("/animal_shelters.json").then((response) => {
        this.animalShelters = response.data;
        console.log(this.animalShelters);
      });
    },
  },
};
</script>

<style></style>
