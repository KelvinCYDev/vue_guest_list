<script setup>
import { reactive } from 'vue';

const data = reactive({
  guests: [{ name: "John", roomNumber: "123" }, { name: "Mary", roomNumber: "222" }],
  newGuest: { name: "", roomNumber: "" },
  editIndex: 0
});

const handleSubmit = () => {
  let list = data.guests.filter((guest) => {
    return guest.name == data.newGuest.name;
  })
  if (list.length > 0) { //Check if guest name exists in the list
    editGuest();
  } else {
    addGuest();
  }
}

const addGuest = () => {
  data.guests.push(data.newGuest);
  data.newGuest = { name: "", roomNumber: "" }
}

const editGuest = () => {
  let guestList = [...data.guests]; //Copy guest list
  guestList = guestList.map((guest) => {
    if (guest.name == data.newGuest.name) {
      guest.roomNumber = data.newGuest.roomNumber;
    }
    return guest;
  })
}

const handleEdit = (name) => {
  let guestList = [...data.guests]; //Copy guest list
  guestList = guestList.filter((guest) => {
    return guest.name == name;
  })
  data.newGuest = JSON.parse(JSON.stringify(guestList[0])); //Push guest data to form to edit by deep copy
}

const deleteGuest = (name) => {
  const confirm = window.confirm('Are you sure you want to delete this guest?');
  if (confirm) {
    data.guests = data.guests.filter((guest) => {
      return guest.name != name;
    })
  }
}

const deleteAllGuest = () => {
  const confirm = window.confirm('Are you sure you want to delete all guests?');
  if (confirm) {
    data.guests = []
  }
}
</script>

<template>
  <section class="bg-blue-50 flex-1">
    <div class="container m-auto max-w-2xl pt-10">
      <div class="bg-white px-6 py-8 mb-4 shadow-md rounded-md border m-4 md:m-0">
        <!--  Add/ Edit Form -->
        <form @submit.prevent="handleSubmit">
          <h2 class="text-2xl text-center font-semibold mb-6">Add Guest</h2>
          <label class="block text-gray-700 font-bold mb-2">Guest Name</label>
          <input v-model="data.newGuest.name" type="text" id="name" name="name"
            class="border rounded w-full py-2 px-3 mb-2" placeholder="John" required />
          <label class="block text-gray-700 font-bold mb-2">Guest room number</label>
          <input v-model="data.newGuest.roomNumber" type="text" id="roomNumber" name="roomNumber"
            class="border rounded w-full py-2 px-3 mb-2" placeholder="123" required />
          <div>
            <button
              class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
              type="submit">
              Add / Edit Guest
            </button>
          </div>
        </form>
        <!--  Guest List -->
        <h3 class="mt-22 text-2xl font-bold text-black mb-6 text-center">
          Guest List
        </h3>
        <div class="grid grid-cols-2 md:grid-cols-3 gap-6">
          <div v-for="guest in data.guests" :key="guest.id" :guest="guest">
            <div class="bg-white rounded-xl shadow-md relative">
              <div class="p-4">
                <div class="mb-6">
                  <div class="text-gray-600 my-2">{{ guest.name }}</div>
                  <h3 class="text-xl font-bold">{{ guest.roomNumber }}</h3>
                  <button @click="handleEdit(guest.name)"
                    class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                    Edit
                  </button>
                  <button @click="deleteGuest(guest.name)"
                    class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                    Delete
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <!--  Delete All Guests -->
        <div class="mt-10">
          <button
            class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline"
            @click="deleteAllGuest()">
            Delete All Guest
          </button>
        </div>
      </div>
    </div>
  </section>
</template>