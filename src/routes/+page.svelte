<script>
  import { onMount } from "svelte";
  import { writable } from "svelte/store";

  let client = writable([]);
  let job = writable([]);
  let office = writable([]);
  let region = writable([]);
  let service = writable([]);

  const storedMap = {
    client: client,
    job: job,
    office: office,
    region: region,
    service: service,
  };

  const apikey =
    "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVnemtvcXl5cnJ0a2lqY215b3dhIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NDMyOTkyMTcsImV4cCI6MjA1ODg3NTIxN30.M19dgayNuzJfuI_UivY8CTlFcSSxPChgsa6DV_0sw4E";

  function getData(tableName) {
    fetch(`https://egzkoqyyrrtkijcmyowa.supabase.co/rest/v1/${tableName}`, {
      method: "GET",
      headers: { apikey: apikey },
    })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        data.
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
      });
  }

  onMount(() => {
    getData("service");
  });
</script>

<div>
  <h1>Supabase Data Fetch Example</h1>
</div>
