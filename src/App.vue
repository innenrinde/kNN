<template>
  <semantic-table
		:columns="columns"
		:rows="rows"
	/>
</template>

<script setup>
import { ref } from "vue";
import SemanticTable from "@/components/SemanticTable.vue";

let columns = [{
	id: "id",
	title: "ID",
  width: "w1",
}, {
	id: "Publisher",
	title: "Publisher",
  width: "w10",
}, {
  id: "Title",
  title: "Title",
  width: "w30",
}, {
  id: "Year",
  title: "Year",
  width: "w10",
}, {
  id: "Notes",
  title: "Notes",
}];

let rows = ref([]);

const xhttp = new XMLHttpRequest();
xhttp.onload = function() {
  rows.value = JSON.parse(this.responseText).data.map(row => {
    return {
      ...row,
      Notes: row.Notes.join(". ")
    };
  });
}
xhttp.open("GET", "https://stephen-king-api.onrender.com/api/books");
xhttp.send();

</script>

<style lang="scss">
@import url(https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap);

html, body {
	padding: 0;
	margin: 0;
	height: 100%;
}

#app {
  font-family: "Poppins", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
	background-color: #fff;
	height: 100%;
	display: flex;
	flex-direction: column;
}
</style>
