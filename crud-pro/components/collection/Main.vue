<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div
    class="row-span-3 bg-gray-50 border-r p-5 rounded-l-lg h-[calc(100vh-150px)] overflow-auto"
  >
    <div class="pb-3 w-[100%]">
      <!-- Adding template to the list -->
      <button
        class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
        @click="addTemplate(templateBody)"
      >
        <span>
          <IconCSS name="material-symbols:add" class="mr-2" size="20" />
        </span>
        Add Template
      </button>
    
    </div>
    <!-- Show list of created templates -->

    <div
      v-for="(template,index) in templateList.data._rawValue"
      v-if="
        templateList.data._rawValue && templateList.data._rawValue.length > 1
      "
      :key="template.name"
      class="border p-4 rounded-md mb-3 shadow-sm bg-white"
    >
      <section @click="prefillTemplateData(template,index)">
        <h5 class="font-[500] text-md mb-2">{{ template.name }}</h5>
        <svg class="h-5 w-5 ml-[14.5rem] mt-[-1.75rem] text-red-500" @click="deleteTemplate(template.uid)" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">  <path stroke="none" d="M0 0h24v24H0z"/>  <line x1="4" y1="7" x2="20" y2="7" />  <line x1="10" y1="11" x2="10" y2="17" />  <line x1="14" y1="11" x2="14" y2="17" />  <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />  <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" /></svg>
        <span class="text-gray-600">{{ template.subject }} - </span>
        <span class="text-gray-600">{{ template.body }}</span>
      </section>
    </div>
    <div
      v-if="
        templateList.data._rawValue && templateList.length == 1
      "
    >
      <span
        ><div class="text-center">
          <svg
            class="mx-auto h-12 w-12 text-gray-400"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            aria-hidden="true"
          >
            <path
              vector-effect="non-scaling-stroke"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M9 13h6m-3-3v6m-9 1V7a2 2 0 012-2h6l2 2h6a2 2 0 012 2v8a2 2 0 01-2 2H5a2 2 0 01-2-2z"
            />
          </svg>
          <h3 class="mt-2 text-sm font-semibold text-gray-900">
            No templates found
          </h3>
          <p class="mt-1 text-sm text-gray-500">
            Get started by creating a new template.
          </p>
        </div>
      </span>
    </div>
  </div>
    <!-- Input field for Template Subject -->
    <div class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]">
      <div class="bg-gray-50 mx-auto px-5 py-3">
        <div class="text-center mb-0 rounded-0">
          <!-- Input field for Template name -->
          <div class="flex justify-between items-center">
            <input
              id="email"
              v-model="templateName"
              type="text"
              name="email"
              class="block rounded-md border-0 py-2 px-3 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[50%]"
              placeholder="Enter Template Name"
            />
          </div>
        </div>
      </div>
      <div class="mx-4 mt-4">
        <input
          id="email"
          v-model="templateSubject"
          type="text"
          name="email"
          class="block mb-3 px-3 rounded-md border-0 py-2 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6 w-[100%]"
          placeholder="Enter Subject"
        />
        <!-- Textarea for template body -->
        <textarea
          v-model="templateBody"
          rows="4"
          class="p-4 h-[calc(100vh-350px)] block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-300 sm:py-1.5 sm:text-sm sm:leading-6"
          placeholder="Add Template Body..."
        />
        <!-- Buttons for template  -->
        <div class="flex justify-end mr-3 mt-4">
          <button
            type="button"
            class="border rounded-md bg-white py-2 px-3 text-sm font-semibold text-gray-600 shadow-sm hover:bg-gray-50 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 mr-3"
          >
            Cancel
          </button>
          <button
            type="button"
            @click="editTemplate()"
            class="rounded-md bg-indigo-600 py-2 px-4 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
          >
            Save
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
// Import vue
import { ref } from "vue";


// Declaring variables
let templateBody = ref("");
let templateName = ref("");
let templateSubject = ref("");
let UId =ref("")

// Get Templates Data from ORM
var templateList = await useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc",''
);

// Prefill data when an existing template is selected
const prefillTemplateData = (data: any,index :Number) => {  
 
  templateName.value = data.name;
  templateBody.value = data.body;
  templateSubject.value = data.subject;

 
  UId.value = data.uid
};

// Edit Prefilled Template Data and Update Template Data
const editTemplate = () =>{
  const putOptions = {
    method: "PUT",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiZGJhZWVkYmQzYmI2NGQ2ZDhkMzYwODhhYzAzM2E0MGEiLCJkIjoiMTY4MDA2NyIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzA5Mzl9.AdgaLskWHSxmOwXM8XDc72OOZAIUYXhdtju83py7C7w`,
    },
    body: {
      project_id: "111",
      name: templateName.value,
      subject: templateSubject.value,
      body: templateBody.value,
      is_active: "1",
      type: "PLAIN_TEXT",
      share_type: "PRIVATE",
      category: "category1",
    },
  };

  const addTemplateData = useAuthLazyFetchPut(
    `https://v1-orm-lib.mars.hipso.cc/email-templates/${UId.value}`,
    putOptions
  );

  templateName.value = "";
  templateBody.value = "";
  templateSubject.value = "";

 
}

// Add Template to the template data
const addTemplate = (data: any) => {
  const postOptions = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiZGJhZWVkYmQzYmI2NGQ2ZDhkMzYwODhhYzAzM2E0MGEiLCJkIjoiMTY4MDA2NyIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzA5Mzl9.AdgaLskWHSxmOwXM8XDc72OOZAIUYXhdtju83py7C7w`,
    },
    body: {
      project_id: "111",
      name: templateName.value,
      subject: templateSubject.value,
      body: data,
      is_active: "1",
      type: "PLAIN_TEXT",
      share_type: "PRIVATE",
      category: "category1",
    },
  };

  const addTemplateData = useAuthLazyFetchPost(
    "https://v1-orm-lib.mars.hipso.cc/email-templates/",
    postOptions
  );

  templateList.data._rawValue.push({
    name: addTemplateData.data._rawValue.name,
    body: addTemplateData.data._rawValue.body,
    subject: addTemplateData.data._rawValue.name.subject,
  });
  templateName.value = "";
  templateBody.value = "";
  templateSubject.value = "";


  useAuthLazyFetch(
  "https://v1-orm-lib.mars.hipso.cc/email-templates/?offset=0&limit=100&sort_column=id&sort_direction=desc",''
);
};

// Delete Template Data
const deleteTemplate =(uid:String,index:Number) =>{
 
    templateList.data._rawValue.splice(index,1)
 
  
  const deleteOptions = {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1IjoiZGJhZWVkYmQzYmI2NGQ2ZDhkMzYwODhhYzAzM2E0MGEiLCJkIjoiMTY4MDA2NyIsInIiOiJzYSIsInAiOiJmcmVlIiwiYSI6ImZpbmRlci5pbyIsImwiOiJ1czEiLCJleHAiOjE2ODMyNzA5Mzl9.AdgaLskWHSxmOwXM8XDc72OOZAIUYXhdtju83py7C7w`,
    },
  };

  const deleteTemplateData = useAuthLazyFetchDelete(
`https://v1-orm-lib.mars.hipso.cc/email-templates/${uid}`,
    deleteOptions
  );

  
}
</script>
