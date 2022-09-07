<script setup>
import { ref, onBeforeMount } from "vue";
import moment from "moment";

import EventDetail from "./buttons/event/EventDetail.vue";
import EventCreate from "./buttons/event/EventCreate.vue";
import EventDelete from "./buttons/event/EventDelete.vue";
import EventNavbar from "./buttons/event/EventNavbar.vue";

const schedules = ref([]);

// GET
const getSchedules = async () => {
  const res = await fetch(`${import.meta.env.BASE_URL}api/events`);
  if (res.status === 200) {
    schedules.value = await res.json();
  } else console.log("error, cannot get data");
};

onBeforeMount(async () => {
  await getSchedules();
});

//DELETE
const removeSchedules = async (id) => {
  if (confirm("Do you really want to delete")) {
    const res = await fetch(`${import.meta.env.BASE_URL}api/events/${id}`,
      {
        method: "DELETE",
      }
    );
    if (res.status === 200) {
      schedules.value = schedules.value.filter(
        (schedules) => schedules.id !== id
      );
      console.log("deleted successfullly");
    } else console.log("error, cannot delete");
  }
};

// PUT
const modifySchedules = async (id, newTime, newNotes, isOverlap) => {
  if (isOverlap) {
  } else {
    const res = await fetch(`${import.meta.env.BASE_URL}api/events/${id}`, {
      method: "PUT",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        eventStartTime: moment(newTime).utcOffset("+07:00"),
        eventNotes: newNotes == null ? null : newNotes.trim(),
      }),
    });
    if (res.status === 200) {
      const edit = await res.json();
      data.value = edit.eventNotes;
      getSchedules();
      console.log("edited successfully");
      console.log(id, newTime, newNotes);
    } else console.log("error, cannot edit");
  }
};

// POST
const createNewSchedules = async (
  Name,
  Email,
  selectedId,
  Time,
  Duration,
  Notes,
  isOverlap
) => {
  console.log(isOverlap);
  if (isOverlap || Name.trim() == "") {
  } else {
    const res = await fetch(`${import.meta.env.BASE_URL}api/events`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify({
        bookingName: Name,
        bookingEmail: Email,
        id: selectedId,
        eventStartTime: moment(Time).utcOffset("+07:00"),
        eventDuration: Duration,
        eventNotes: Notes.trim() == "" ? null : Notes.trim(),
      }),
    });
    if (res.status === 201) {
      getSchedules();
    } else console.log("error, cannot be added");
  }
};

const currentDetail = ref({});
const data = ref("");

const moreDetail = (curbookingId) => {
  currentDetail.value = curbookingId;
  data.value = curbookingId.eventNotes;
  currentDetail.value.eventStartTime = moment(
    currentDetail.value.eventStartTime
  ).format("YYYY-MM-DDTHH:mm:ss");
  getSchedules();
};

const filter = ref();
const upcomingEvent = ref();
const pastEvent = ref();
const getClinic = async (id) => {
  if (id !== 0) {
    upcomingEvent.value = undefined;
    pastEvent.value = undefined;
    const res = await fetch(
      import.meta.env.VITE_CATEGORY_URL + "/" + id + "/events"
    );
    if (res.status === 200) {
      filter.value = await res.json();
      console.log(filter.value);
    } else console.log("error, cannot get data");
  } else {
    filter.value = undefined;
    upcomingEvent.value = undefined;
    pastEvent.value = undefined;
  }
};

const getUpcoming = async () => {
  const res = await fetch(import.meta.env.VITE_EVENT_URL + "/upcoming");
  if (res.status === 200) {
    filter.value = await res.json();
    upcomingEvent.value = filter.value;
    pastEvent.value = undefined;
    console.log(filter.value);
  } else console.log("error, cannot get data");
};

const getPast = async () => {
  const res = await fetch(import.meta.env.VITE_EVENT_URL + "/past");
  if (res.status === 200) {
    filter.value = await res.json();
    pastEvent.value = filter.value;
    upcomingEvent.value = undefined;
    console.log(filter.value);
  } else console.log("error, cannot get data");
};
</script>

<template>
  <div id="contents-list" class="px-10 py-5 flex justify-center">
    <table class="table-zebra table-layout table-element">
      <thead class="table-header bg-base-200">
        <tr>
          <EventNavbar @option="getClinic" @upcoming="getUpcoming" @past="getPast" />
          <th>
            <EventCreate :detail="schedules" @create="createNewSchedules" />
          </th>
        </tr>
      </thead>
      <div v-if="schedules < 1 || filter < 1" class="no-event text-5xl pt-20">
        <p v-if="upcomingEvent == undefined && pastEvent == undefined">
          No Scheduled Events
        </p>
        <p v-else-if="upcomingEvent != undefined">
          No On-Going or Upcoming Events
        </p>
        <p v-else>No Past Events</p>
      </div>
      <tbody v-else>
        <tr v-if="filter == undefined" v-for="contents in schedules" :key="contents.id">
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.bookingName }}
            </div>
          </td>
          <td class="p-10 text-xl">
            <div class="pt-2">
              {{ contents.eventCategory.eventCategoryName }}
            </div>
          </td>

          <td class="p-10 text-xl">
            {{
                moment(contents.eventStartTime)
                  .local()
                  .format("D MMMM YYYY, h:mm:ss A")
            }}
          </td>

          <td class="p-10 text-xl">{{ contents.eventDuration }} minute</td>

          <td>
            <div id="showDetail">
              <EventDetail @moreDetail="moreDetail(contents)" :detail="currentDetail" :data="data" :event="schedules"
                @editDetail="modifySchedules" />

              <EventDelete @delete="removeSchedules(contents.id)" />
            </div>
          </td>
        </tr>
        <tr v-else v-for="contents in filter">
          <td class="p-10 text-xl">
            <div class="box-element break-words">
              {{ contents.bookingName }}
            </div>
          </td>
          <td class="p-10 text-xl">
            <div class="pt-2">
              {{ contents.eventCategory.eventCategoryName }}
            </div>
          </td>

          <td class="p-10 text-xl">
            {{
                moment(contents.eventStartTime)
                  .local()
                  .format("D MMMM YYYY, h:mm:ss A")
            }}
          </td>

          <td class="p-10 text-xl">{{ contents.eventDuration }} minute</td>

          <td>
            <div id="showDetail">
              <Detail @moreDetail="moreDetail(contents)" :detail="currentDetail" :data="data"
                @editDetail="modifySchedules" />

              <Delete @delete="removeSchedules(contents.id)" />
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.no-event {
  text-align: center;
  width: 100%;
  position: absolute;
  z-index: -1;
}

table {
  text-align: left;
  position: relative;
  border-collapse: collapse;
  border-radius: 6px;
}

input,
textarea {
  color: rgb(0 0 0);
}

.table-header {
  position: sticky;
  top: 0;
  height: 100px;
}

.table-layout {
  table-layout: fixed;
  width: 90%;
}

.box-element {
  width: 250px;
}

.table-element {
  height: 100px;
}
</style>
