<script lang="ts">
    import PageTitle from "../components/PageTitle.svelte";
    import { onMount } from "svelte";
    import { Api, type WeatherForecast } from "../../Api";
    import { config } from "../../config";

    let forecasts : Array<WeatherForecast> = [];

    async function retrieveAsync() {
        const api = new Api();
        api.baseUrl = config.apiEndpoint;
        
        const response = await api.weatherForecast.weatherForecastList();
        
        if (response.status == 200)
        {
            forecasts = response.data;
        }
    }
    onMount(async () =>
    {
        await retrieveAsync();
    });
</script>

<PageTitle>Weather forecast</PageTitle>

<h1>Weather forecast</h1>

<p>This component demonstrates fetching data from the server.</p>

<div class="">
    <button type="button" class="btn btn-primary" on:click={retrieveAsync}>Refresh</button>
</div>

{#if forecasts == null}
    <p><em>Loading...</em></p>
{:else}
    <table class="table">
        <thead>
            <tr>
                <th>Date</th>
                <th>Temp. (C)</th>
                <th>Temp. (F)</th>
                <th>Summary</th>
            </tr>
        </thead>
        <tbody>
            {#each forecasts as forecast}
                <tr>
                    <td>{new Date(forecast.date ?? "").toLocaleDateString()}</td>
                    <td>{forecast.temperatureC}</td>
                    <td>{forecast.temperatureF}</td>
                    <td>{forecast.summary}</td>
                </tr>
            {/each}
        </tbody>
    </table>
{/if}
