<h2>Server monitoring</h2>
<h3 id='monitoring-install'>Getting started with the Amon agent</h3>
<p>The Amon Agent is a small piece of software that runs on your hosts. Its job is to collect metrics and bring them to Amon so that you can do something useful with your
monitoring and performance data.
</p>
<h3>Installing the agent</h3>
<p>
	Amon has a <a href="https://github.com/amonapp/amonagent-go">single binary collector agent</a>, written in Go.
	<br><br>
	By default you can install it an all your servers with one line, displayed on your Servers page
</p>

<pre><code class='language-bash'>API_KEY=unique_api_key bash -c "$(curl -L http://youramoninstance/install/)"</code></pre>
<p>This will install the APT/Yum packages for the Amon Agent and will prompt you for your password.
<br><br>
	By default the script installs:
</p>
<ol>
	<li>
	    The Amon agent to /opt/amonagent/amonagent and /usr/bin/amonagent (You can call amonagent from anywhere)
	</li>
</ol>
<h3 id='monitoring-config'>Agent configuration</h3>
<p>The Amon agent has a single configuration file , which you can find at <span class="code">/etc/opt/amonagent/amonagent.conf</span>.
The configuration file is in JSON, so after editing it, please check if the syntax is valid at:
<a href="http://jsonlint.com/">http://jsonlint.com/</a>.
<br><br>
The configuration file has the following parameters:
</p>
<ul>
	<li>
	  <strong>api_key</strong> - valid API key.
	</li>
	<li>
	  <strong>amon_instance</strong> - The IP Address where your Amon instance is running. This is where the agent will send data.
	</li>
	<li>
	  <strong>interval</strong> - (Optional) How often to gather metrics. In seconds, defaults to 60 seconds.
	</li>

</ul>
<p>Below you can see a complete configuration file with all the options:</p>
<pre><code class="language-bash">{
"api_key": "your-api-key",
"amon_instance": "https://youramoninstance",
"interval": 60
}
</code></pre>

<h3 id='monitoring-running'>Running the agent</h3>
<ul>
<li>On sysv systems, the amonagent daemon can be controlled via <code class="language-bash">service amonagent [action]</code>
 </li>
 <li>On systemd systems (such as Ubuntu 15+), the amonagent daemon can be controlled via <code class="language-bash">systemctl [action] amonagent</code>
 </li>
 </ul>
<p>


<h3 id='monitoring-testing'>Testing the agent</h3>

<p>
The agent executable is located in
<span class="code">/opt/amonagent/amonagent(/usr/bin/amonagent)</span> and accepts the following commands: </p>
<ul>
  <li>
        <code class="language-bash">$ amonagent -test</code> - Collects all system metrics and displays the results in the terminal.
  </li>
</ul>

<h3 id='monitoring-manual-install'>Manually installing the agent</h3>
<p>If the automatic installer fails, you can easily install the agent manually by pasting the following commands in the terminal:
</p>
<pre ><code class="language-bash">
# On Debian/Ubuntu
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv AD53961F
echo "deb http://packages.amon.cx/repo amon contrib" | sudo tee /etc/apt/sources.list.d/amon.list
sudo apt-get update
sudo apt-get install amonagent
echo '{
	"api_key":"your api-key",
	"amon_instance": "http://your amon instance IP"
}' > /etc/opt/amonagent/amonagent.conf

sudo service amonagent restart (or) sudo systemctl restart amonagent

# On Red Hat/Fedora/CentOS
echo -e '[amon]\nname = Amon.\nbaseurl = http://packages.amon.cx/rpm/\nenabled=1\ngpgcheck=0\npriority=1' > /etc/yum.repos.d/amon.repo
sudo yum -y install amonagent

echo '{
	"api_key":"your api-key",
	"amon_instance": "http://your amon instance IP"
}' > /etc/opt/amonagent/amonagent.conf

sudo service amonagent restart (or) sudo systemctl restart amonagent
</code></pre>


<h3 id='monitoring-issues'>Issues getting the Agent installed</h3>
<p>
	If you encountered an issue during the Agent installation please reach out to <a href="mailto:martin@amon.cx">martin@amon.cx</a>
	with the contents of <span class="code">amonagent-install.log</span>
</p>
<h3>Issues getting the Agent reporting</h3>
<p>
If you get the Agent installed but you are not seeing any data in Amon, you can troubleshoot in the following manner. <br>
First, run the <strong>status</strong> command - <code class="language-bash">sudo service amonagent status (or) sudo systemctl status amonagent</code>.
<br><br>
If the agent is running, check if the Amon API is accessible from your server:
</p>
<pre class='language-bash'><code>$ amonagent -test</code>
</pre>
<p>
<br>
You should also check the logs: <span class="code">/var/log/amonagent.log</span>. Errors in the logs may also reveal the cause of any issues.
<br><br>
If non of these steps solve the problem, please send the logs as attachment to support[at]amon.cx.
</p>

