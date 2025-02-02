<template>
	<div class="container">
		<div class="header-bar">
      Keywords =
			<input
        class="keywords"
				type="text"
				placeholder="type here a phrase and the Enter..."
        v-model="query"
        @keydown="onQueryEnter"
			/>
      <span>k =</span>
      <input
        class="knn"
        type="text"
        placeholder="define k number"
        v-model="kNumber"
        @keydown="onKNumberEnter"
    />
		</div>

		<div class="table-section">
			<div class="table">
				<div
					class="header"
				>
					<div
						v-for="column in columns"
						:key="column"
            :class="column.width"
					>
						{{ column.title }}
					</div>
				</div>

				<div
					v-for="row in localRows"
					:key="row"
					class="content"
				>
					<div
						v-for="column in columns"
						:key="column"
					>
						{{ row[column.id] }}
					</div>
				</div>

			</div>
		</div>

	</div>

</template>

<script setup>
import { defineProps, defineModel, toRefs, reactive, watch } from "vue";
import { kNNSearch } from "@/components/kNNSearch";

const props = defineProps({
	columns: Array,
	rows: Array
});

const query = defineModel("vector");
const kNumber = defineModel({ default: 100 });

const { columns, rows } = toRefs(props);

let localRows = [];
watch(rows, async (data) => {
  localRows = reactive([...data]);
});

let kNN = new kNNSearch();
kNN.setK(kNumber.value);

/**
 * Get filtered rows
 */
const applyResults = () => {
  let results = kNN.applySearch(rows.value, columns.value);
  localRows.splice(0);
  results.forEach(row => localRows.push(row));
}

/**
 * On change keywords
 * @param event
 */
let onQueryEnter = (event) => {
  if (event.keyCode === 13) {
    kNN.setVector(query.value);
    applyResults();
  }
};

/**
 * On change k number
 * @param event
 */
let onKNumberEnter = (event) => {
  if (event.keyCode === 13) {
    kNN.setK(kNumber.value);
    applyResults();
  }
};

</script>

<style lang="scss">
.container {
	font-size: 12px;
	height: 100%;
	display: flex;
	flex-direction: column;
}

.header-bar {
  padding: 20px;

  input.keywords,
  input.knn {
    padding: 7px;
    border: solid 1px #c3c3c3;
    border-radius: 5px;
    box-shadow: #cdcdcd 1px 1px 3px;
  }

  input.keywords {
    width: 30%;
  }

  input.knn {
    width: 50px;
  }

  span {
    display: inline-block;
    margin: 0 5px 0 50px;
  }
}

.table-section {
  display: flex;
  flex-direction: column;
  overflow: auto;
  margin: 0 20px 20px 20px;
  border-bottom: solid 1px #dadada;

  .table {
    display: table;
    text-align: left;
    box-shadow: #cdcdcd 0 0 3px;
    border: solid 1px #dadada;
    border-top: none;
    border-bottom: none;

    .header {
      display: table-row;
      position: sticky;
      top: 0;

      > div {
        display: table-cell;
        background-color: #f3f4f6;
        border-top: solid 1px #dadada;
        border-bottom: solid 1px #dadada;
        font-weight: 600;
        color: #35a5da;
        padding: 7px;
      }

      .w1 {
        width: 1%;
      }

      .w10 {
        width: 10%;
      }

      .w30 {
        width: 30%;
      }
    }

    .content {
      display: table-row;

      &:nth-child(odd) > div {
        background-color: #f0fdf4;
      }

      &:hover > div {
        background-color: #4bb0df;
        color: #fff;
      }

      > div {
        display: table-cell;
        padding: 7px;
      }
    }
  }
}

</style>