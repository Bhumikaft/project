<template>
    <div class="max-w-3xl py-12 mx-auto">
      <div class="flex flex-row items-center justify-between">
        <h2 class="text-5xl font-black text-gray-900">Lists</h2>
        <!-- <Button icon-left="plus">New List</Button> -->
      </div>
  
      <div class="mt-2"> 
        <Card title="General">
          <div>
            <ul>
              <li class="flex flex-row space-y-2 items-center justify-between" v-for="action in actions.data" :key="action.title">
                <span>{{action.title}}</span>
              </li>
            </ul>
            <Button @click="addActionDialogShown = true" icon-left="plus"> New Action</Button>
          </div>
        </Card>
      </div>
    </div>
  
    <Dialog 
      :options="{
        title: 'Add New Action',
        actions: [
          {
            label: 'Add New Action',
            appearance: 'primary',
            handler: ({ close }) => {
              addAction();
              close();
            },
          },
          { label: 'Cancel' },
        ]
      }"
      v-model="addActionDialogShown"
    >
      <template #body-content>
        <div>
          <Input type="text" required placeholder="Title" label="Title"/>
  

        </div>
      </template>
    </Dialog>
  </template>
  
  <script setup>
  import { Dialog, createListResource, Card } from 'frappe-ui';
  import { reactive, ref } from 'vue';
  
  const action = reactive({
    title: ''
  });
  const addActionDialogShown = ref(false);
  const actions = createListResource({
    doctype: 'Action',
    fields: ["title", "date", "status"],
    filters: {
      status: 'Completed'
    },
    limit: 10
  });
  actions.reload();
  
  const addAction = () => {
    actions.insert.submit(action);
  };
  </script>
  