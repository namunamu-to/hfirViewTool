<script setup lang="ts">
import fhirConf from './fhirConfig.json';
import fhirJson from './fhir.json'
import { createApp } from 'vue';
import App from './app.vue';
import row from './components/row.vue';
import toolbar from './components/toolbar.vue';
import PrimeVue from 'primevue/config';
import Aura from '@primevue/themes/aura';
import 'primeicons/primeicons.css'
import Row from 'primevue/row';
import Tree from 'primevue/tree';

const app = createApp(App);
app.use(PrimeVue, {
  theme: {
    preset: Aura
  }
});

let fhirEntrysJson = {
  "Composition": [],
  "Patient": [],
  "Practitioner": [],
  "Organization": [],
  "Encounter": [],
  "Condition": [],
  "Observation": [],
  "AllergyIntolerance": [],
  "FamilyMemberHistory": [],
  "MedicationRequest": [],
  "Bundle": [],
  "CarePlan": [],
  "Procedure": [],
  "ImagingStudy": [],
  "DiagnosticReport": [],
  "Device": [],
  "DeviceUseStatement": [],
  "Immunization": [],
  "Consent": [],
  "RelatedPerson": [],
  "ResearchStudy": [],
  "ResearchSubject": [],
  "DocumentReference": []
};

let resourceTypes = Object.keys(fhirEntrysJson);

function readJsonEntrys() {
  let entrys = fhirJson.entry;
  for (let entry of entrys) {
    const rType = entry["resource"]["resourceType"];
    fhirEntrysJson[rType].push(entry);
  }
}

readJsonEntrys();
let treeData: any = parseTreeData(fhirEntrysJson);

function getKeyFmts(json: any) {
  let keyFmtBuf: any = [];
  setKeyFmts(json);
  function setKeyFmts(json: any, keyFmt: string = "") {
    if (typeof json != 'object' || json == null) return;

    for (let key in json) {
      let tmpKeyFmt = keyFmt;

      if (tmpKeyFmt == "") tmpKeyFmt = key;
      else tmpKeyFmt += "-" + key

      if (typeof json[key] != "object") keyFmtBuf.push(tmpKeyFmt); //最終的な値だけ
      // keyFmtBuf.push(tmpKeyFmt);
      setKeyFmts(json[key], tmpKeyFmt);
    }
  }

  return keyFmtBuf;
}


// function getJsonRef(json: any, keyFmt: any) {
//   const keys = keyFmt.split("-");
//   let lastKey = "";

//   for (let key of keys) {
//     if (!isNaN(key)) key = parseInt(key);
//     if (typeof json[key] != "object") break;

//     json = json[key];
//     lastKey = key;
//   }

//   return json;
// }

// function getJsonByKeyFmt(json: any, keyFmt: any) {
//   const keys = keyFmt.split("-");
//   let key = keys[keys.length - 1];
//   let result = getJsonRef(json, keyFmt)[key];

//   return result;
// }

// function setJsonByKeyFmt(json: any, keyFmt: any, setValue: any) {
//   const keys = keyFmt.split("-");
//   let key = keys[keys.length - 1];
//   let ref = getJsonRef(json, keyFmt);
//   ref[key] = setValue;
// }



function parseTreeData(json: any, nowNumKey: any = "") {
  let result = [];

  for (let key in json) {
    if (nowNumKey == "") nowNumKey = result.length;
    else nowNumKey += "-" + result.length;

    let addData: any = {
      "key": nowNumKey,
    };


    if (typeof json[key] != "object") {
      addData["label"] = key + " : " + json[key];
      result.push(addData);
      continue;
    }

    addData["label"] = key;
    addData["children"] = parseTreeData(json[key], nowNumKey);
    result.push(addData);

  }

  return result;
}

</script>

<template>
  <h1>紹介状情報</h1>
  <Tree :value="treeData" class="w-full md:w-[30rem]"></Tree>
</template>

<style>
TabList {
  display: inline-block;
}
</style>