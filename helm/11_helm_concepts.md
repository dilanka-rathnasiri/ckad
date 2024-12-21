# Helm Concepts

* Helm Charts consists of
  * Template files
  * `values.yaml` file
    * Values for the variables
    * Some variables may have default values
  * `Chart.yaml` file
    * Metadata of the chart
* Each helm chart installation is called a **release**
  * The same helm chart can be installed multiple times as different releases
  * Each release has its own release name
* Helm charts is downloaded as a `tar` archive file

## Helm Commands

<table>
<tr>
<th>Command</th>
<th>Description</th>
</tr>

<tr>
<td><code>helm upgrade</code></td>
<td>Upgrade release to a new revision (version) of a chart</td>
</tr>

<tr>
<td><code>helm rollback</code></td>
<td>
<ul>
<li>Rollback to a previous revision (version) </li>
<li>If the previous revision isn't specified => rollback to the last previous revision</li>
</ul>
</td>
</tr>
</table>