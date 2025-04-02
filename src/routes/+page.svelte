<script>
  import { onMount } from "svelte";
  import { writable, get } from "svelte/store";

  let client = writable([]);
  let job = writable([]);
  let office = writable([]);
  let region = writable([]);
  let service = writable([]);

  const storeMap = {
    client: client,
    job: job,
    office: office,
    region: region,
    service: service,
  };

  let options = ["client", "job", "office", "region", "service"];

  // Initialize selectedOption with empty string
  let selectedOption = "";

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

        const store = storeMap[tableName];

        if (store) {
          store.set(data);
        } else {
          console.error("Error fetching data");
        }
      })
      .catch((error) => {
        console.error("Error fetching data:", error);
      });
  }

  // Add function to handle button click
  function handleGetData() {
    if (selectedOption) {
      getData(selectedOption);
      console.log(`Fetching data for: ${selectedOption}`);
    } else {
      console.log("Please select a table first");
    }
  }

  // Get the current value of the selected store
  $: currentData = selectedOption ? get(storeMap[selectedOption]) : [];

  onMount(() => {
    getData("service");
    getData("job");
  });
</script>

<div class="grid-container">
  <div class="header">
    <h1 class="title">DATA<span class="accent">STREAM</span></h1>
  </div>

  <div class="control-panel">
    <div class="select-container">
      <select bind:value={selectedOption} class="custom-select">
        <option value="">SELECT DATABASE</option>
        {#each options as option}
          <option value={option}>{option.toUpperCase()}</option>
        {/each}
      </select>
      <button
        class="fetch-button"
        on:click={handleGetData}
        disabled={!selectedOption}
      >
        <span class="button-text">ACCESS DATA</span>
      </button>
    </div>
  </div>

  {#if selectedOption}
    <div class="data-container">
      <div class="data-header">
        <h2>{selectedOption.toUpperCase()} <span class="accent">DATA</span></h2>
      </div>
      {#if currentData?.length > 0}
        <ul class="data-list">
          {#each currentData as item}
            <li class="data-item">
              <div class="item-inner">
                {#each Object.entries(item) as [key, value]}
                  <div class="item-property">
                    <span class="property-key">{key}</span>
                    <span class="property-value">{value}</span>
                  </div>
                {/each}
              </div>
            </li>
          {/each}
        </ul>
      {:else}
        <div class="empty-state">
          <p>
            NO DATA AVAILABLE IN <span class="accent"
              >{selectedOption.toUpperCase()}</span
            >
          </p>
        </div>
      {/if}
    </div>
  {/if}
</div>

<style>
  /* Global Styles */
  :global(body) {
    background-color: #000000;
    color: #ffffff;
    font-family: "Rajdhani", sans-serif;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
    /* Removed checker background */
    background-image: radial-gradient(
      circle at 50% 50%,
      rgba(184, 41, 255, 0.05) 0%,
      rgba(0, 0, 0, 0) 75%
    );
    background-size: 100% 100%;
  }

  .grid-container {
    width: 100%;
    max-width: 1200px;
    margin: 40px auto;
    padding: 0 20px;
    position: relative;
  }

  /* Header Styles */
  .header {
    text-align: center;
    margin-bottom: 50px;
    position: relative;
  }

  .header::after {
    content: "";
    position: absolute;
    bottom: -15px;
    left: 25%;
    width: 50%;
    height: 2px;
    background: linear-gradient(
      90deg,
      rgba(184, 41, 255, 0),
      rgba(184, 41, 255, 1) 20%,
      rgba(184, 41, 255, 1) 80%,
      rgba(184, 41, 255, 0)
    );
    box-shadow: 0 0 20px 0 #b829ff;
  }

  .title {
    font-size: 48px;
    letter-spacing: 10px;
    text-transform: uppercase;
    color: #ffffff;
    margin: 0;
    text-shadow:
      0 0 10px #b829ff,
      0 0 20px #b829ff,
      0 0 30px #b829ff;
    font-weight: 700;
  }

  .accent {
    color: #b829ff;
    font-weight: 300;
  }

  /* Control Panel Styles */
  .control-panel {
    display: flex;
    justify-content: center;
    margin-bottom: 50px;
    perspective: 1000px;
  }

  .select-container {
    width: 400px;
    border: 1px solid #b829ff;
    background: rgba(0, 0, 0, 0.7);
    padding: 25px;
    box-shadow:
      0 0 20px rgba(184, 41, 255, 0.3),
      inset 0 0 10px rgba(184, 41, 255, 0.1);
    position: relative;
    animation: glow 4s infinite alternate;
  }

  .select-container::before,
  .select-container::after {
    content: "";
    position: absolute;
    background: #b829ff;
  }

  .select-container::before {
    top: -1px;
    left: 20px;
    width: 60%;
    height: 1px;
    box-shadow: 0 0 10px 1px #b829ff;
  }

  .select-container::after {
    bottom: -1px;
    right: 20px;
    width: 60%;
    height: 1px;
    box-shadow: 0 0 10px 1px #b829ff;
  }

  .custom-select {
    width: 100%;
    padding: 15px 20px;
    background-color: rgba(0, 0, 0, 0.7);
    color: #b829ff;
    border: 1px solid #b829ff;
    font-size: 18px;
    letter-spacing: 2px;
    text-transform: uppercase;
    cursor: pointer;
    appearance: none;
    -webkit-appearance: none;
    transition: all 0.3s ease;
    box-shadow: 0 0 10px rgba(184, 41, 255, 0.2);
    font-family: "Rajdhani", sans-serif;
    font-weight: 600;
    position: relative;
  }

  .custom-select:focus {
    outline: none;
    box-shadow: 0 0 20px rgba(184, 41, 255, 0.4);
  }

  .custom-select option {
    background-color: #000000;
    color: #b829ff;
    padding: 10px;
    font-weight: 600;
  }

  .fetch-button {
    width: 100%;
    margin-top: 25px;
    padding: 18px;
    background: transparent;
    color: #b829ff;
    border: 1px solid #b829ff;
    font-size: 20px;
    font-weight: 600;
    letter-spacing: 3px;
    text-transform: uppercase;
    cursor: pointer;
    position: relative;
    overflow: hidden;
    transition: all 0.3s ease;
    font-family: "Rajdhani", sans-serif;
    text-shadow: 0 0 5px #b829ff;
    box-shadow: 0 0 15px rgba(184, 41, 255, 0.3);
  }

  .fetch-button::before {
    content: "";
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(
      90deg,
      transparent,
      rgba(184, 41, 255, 0.2),
      transparent
    );
    transition: all 0.6s ease;
  }

  .fetch-button:hover:not(:disabled)::before {
    left: 100%;
  }

  .fetch-button:hover:not(:disabled) {
    background-color: rgba(184, 41, 255, 0.1);
    box-shadow: 0 0 20px rgba(184, 41, 255, 0.5);
    text-shadow: 0 0 10px #b829ff;
  }

  .fetch-button:disabled {
    border-color: #371a4a;
    color: #371a4a;
    cursor: not-allowed;
    box-shadow: none;
  }

  .button-text {
    position: relative;
    z-index: 1;
  }

  /* Data Container Styles */
  .data-container {
    background-color: rgba(0, 0, 0, 0.7);
    border: 1px solid #b829ff;
    box-shadow: 0 0 20px rgba(184, 41, 255, 0.2);
    padding: 25px;
    margin-top: 20px;
    position: relative;
  }

  .data-container::before,
  .data-container::after {
    content: "";
    position: absolute;
    background: #b829ff;
  }

  .data-container::before {
    top: -1px;
    left: 40px;
    width: 40%;
    height: 1px;
    box-shadow: 0 0 10px 1px #b829ff;
  }

  .data-container::after {
    bottom: -1px;
    right: 40px;
    width: 40%;
    height: 1px;
    box-shadow: 0 0 10px 1px #b829ff;
  }

  .data-header {
    border-bottom: 1px solid rgba(184, 41, 255, 0.3);
    padding-bottom: 15px;
    margin-bottom: 20px;
  }

  .data-header h2 {
    font-size: 28px;
    letter-spacing: 4px;
    color: #ffffff;
    margin: 0;
    text-shadow: 0 0 10px rgba(184, 41, 255, 0.7);
    text-transform: uppercase;
  }

  .data-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .data-item {
    margin-bottom: 20px;
    border-left: 2px solid #b829ff;
    background-color: rgba(12, 0, 24, 0.4);
    transition: all 0.3s ease;
    position: relative;
    box-shadow: 0 0 10px rgba(184, 41, 255, 0.1);
  }

  .data-item:hover {
    transform: translateX(5px);
    background-color: rgba(24, 0, 40, 0.5);
    box-shadow: 0 0 15px rgba(184, 41, 255, 0.2);
  }

  .data-item::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 3px;
    height: 100%;
    background: #b829ff;
    box-shadow: 0 0 10px 1px #b829ff;
    opacity: 0.8;
  }

  .item-inner {
    padding: 20px;
  }

  .item-property {
    display: flex;
    justify-content: space-between;
    margin: 8px 0;
    padding: 8px 0;
    border-bottom: 1px solid rgba(184, 41, 255, 0.1);
  }

  .property-key {
    color: #b829ff;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    flex: 0 0 30%;
    position: relative;
    text-shadow: 0 0 5px rgba(184, 41, 255, 0.5);
  }

  .property-value {
    color: #ffffff;
    flex: 0 0 65%;
    text-align: right;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .empty-state {
    padding: 50px 0;
    text-align: center;
  }

  .empty-state p {
    color: #4a6b80;
    font-size: 18px;
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  /* Animation for glowing effect */
  @keyframes glow {
    0% {
      box-shadow:
        0 0 10px rgba(184, 41, 255, 0.2),
        inset 0 0 5px rgba(184, 41, 255, 0.1);
    }
    100% {
      box-shadow:
        0 0 20px rgba(184, 41, 255, 0.4),
        inset 0 0 10px rgba(184, 41, 255, 0.2);
    }
  }

  /* Responsive adjustments */
  @media (max-width: 768px) {
    .select-container {
      width: 90%;
    }
  }
</style>
