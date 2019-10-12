<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome file</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#configuration-on-naas">Configuration on naas</a></li>
<li><a href="#naas">1. NAAS</a>
<ul>
<li><a href="#basic-info">1.1 Basic Info</a></li>
<li><a href="#configuration">1.2 Configuration</a></li>
<li><a href="#multiple-subnets-in-one-vpc">1.3 Multiple subnets in one VPC</a></li>
<li><a href="#basic-info-1">2.1 Basic Info</a></li>
<li><a href="#configuration-1">2.2 Configuration</a></li>
</ul>
</li>
<li><a href="#appendix--1.-create-vpcroutes">Appendix- 1. Create VPC/Routes</a>
<ul>
<li></li>
</ul>
</li>
<li><a href="#appendix-2.-share-image-to-customer-target-om-team">Appendix-2. Share image to customer (Target: O&M team)</a></li>
</ul>

    </div>
  </div>
  <div class="stackedit__right">
    <div class="stackedit__html">
      <header> COnCOn</header>
<h1 id="configuration-on-naas">Configuration on naas</h1>

<table>
<thead>
<tr>
<th>Version</th>
<th>Release Date</th>
<th>Author</th>
<th>ChangeLog</th>
</tr>
</thead>
<tbody>
<tr>
<td>1.0</td>
<td>2019-05-28</td>
<td>Katherine Xu</td>
<td>First version</td>
</tr>
<tr>
<td>2.0</td>
<td>2019-06-10</td>
<td>Katherine Xu</td>
<td>Update security group part and add disable “check source/destination ip” for vaer</td>
</tr>
<tr>
<td>2.1</td>
<td>2019-06-12</td>
<td>Katherine Xu</td>
<td>Add function to share image to customer account</td>
</tr>
<tr>
<td>2.2</td>
<td>2019-06-24</td>
<td>Katherine Xu</td>
<td>Add operation for multiple subnets in the same VPC for NAAS</td>
</tr>
<tr>
<td>—</td>
<td>—</td>
<td>—</td>
<td>—</td>
</tr>
</tbody>
</table><h1 id="naas">1. NAAS</h1>
<h2 id="basic-info">1.1 Basic Info</h2>
<h3 id="topology">1.1.1 Topology</h3>
<p><img src="resources/FD6A5F73DC42037930EA7F44C01DC3AF.jpg" alt="IMAGE" width="598" height="367"></p>
<ul>
<li>For different VPC, for the restriction of AWS VPC that can only route the subnets in the two VPCs, so for each VPC, we need to locate an vAER for it.</li>
</ul>
<h3 id="scenarios-support">1.1.2 Scenarios support</h3>

<table>
<thead>
<tr>
<th>VPC</th>
<th>Subnets</th>
<th>AWS Support Status</th>
</tr>
</thead>
<tbody>
<tr>
<td>1 VPC</td>
<td>Private networks</td>
<td>Yes</td>
</tr>
<tr>
<td>1 VPC</td>
<td>Public networks</td>
<td>Yes</td>
</tr>
<tr>
<td>Multiple VPCs</td>
<td>Private networks</td>
<td>Yes, but each VCP need 1vAER</td>
</tr>
<tr>
<td>Multiple VPCs</td>
<td>Public networks</td>
<td>Yes, but each VCP need 1vAER</td>
</tr>
</tbody>
</table><h2 id="configuration">1.2 Configuration</h2>
<p>For each VPC, you need to create a vAER for it. The configuration are the same. This guide use 1 vpc as an example.</p>
<h3 id="create-a-vpc">1.2.1 Create a VPC</h3>
<blockquote>
<p>Skip this part if the VPC already existed<br>
Check the configuration in Appendix-1</p>
</blockquote>
<h3 id="create-vaer-in-vpc">1.2.2 Create vAER in VPC</h3>
<blockquote>
<p>Before below steps, ensure the vAER AMI has been shared by AXESDN.<br>
Check the configuration in Appendix-2 for share image to customer</p>
</blockquote>
<p>Click “启动EC2实例” on AWS console vpc page</p>
<ul>
<li>
<p>On “1. 选择 AMI” page ，choose 所有权 “与我共享”， click “选择”<br>
<img src="resources/DD3CADD178F8A0CC8E47F71017D91B8E.jpg" alt="IMAGE" width="1273" height="477"></p>
</li>
<li>
<p>On “2: 选择一个实例类型”, choose t2.micor(1 vCPU and 1GiB), click “下一步：配置实例详细信息”<br>
<img src="resources/FA8594F3ED120B6EC4655E0E85602D0B.jpg" alt="IMAGE" width="1315" height="462"></p>
</li>
<li>
<p>On “3: 配置实例详细信息”, choose “网络”, on “自动分配公有IP”，choose “启用”， click “下一步：添加存储”<br>
<img src="resources/CB2DC0E07B45168E719003FB852F85C4.jpg" alt="IMAGE" width="1318" height="892"></p>
</li>
<li>
<p>On “4: 添加存储”, use the default "10"GB disk click “下一步：添加标签”<br>
<img src="resources/8F24EA4F4FEA99C641910BD854F83675.jpg" alt="IMAGE" width="1321" height="438"></p>
</li>
<li>
<p>On “5: 添加标签”, add the label for better operation and click “下一步：配置安全组”<br>
<img src="resources/0DB5CFA018A32C25CBE6BA57CAA5EB39.jpg" alt="IMAGE" width="1316" height="429"></p>
</li>
<li>
<p>On “6: 配置安全组”, choose “创建一个新的安全组”， add below rules，and click “审核和启动”<br>
Below rules should be added for vAER:</p>
</li>
<li>
<p>tcp-22(ssh)</p>
</li>
<li>
<p>tcp-8000(aos-agent)</p>
</li>
<li>
<p>icmp</p>
</li>
<li>
<p>all the traffic from the same vAER network</p>
</li>
</ul>
<p>Below is the basic config without monitor function, get the &lt;ip_network&gt; part according to the vAER location. Make it easy but less secure you can use 0.0.0.0/0.<br>
<img src="resources/AC1764630320E57AC9D11F3CC66389E2.jpg" alt="IMAGE" width="1742" height="619"></p>
<blockquote>
<p>For the IP address of each rule with ‘0.0.0.0/0’, ask O&amp;M team for a specific IP network.</p>
</blockquote>
<ul>
<li>On “7: 核查实例启动”, check the configuration and click “启动”.</li>
</ul>
<blockquote>
<p>“AWS” may ask you to choose a key for the new EC2, just choose one, vAER has its own login method.</p>
</blockquote>
<ul>
<li>vAER creation finished.</li>
</ul>
<h3 id="bond-a-floating-ip-to-the-vaer">1.2.3 Bond a floating IP to the vAER</h3>
<p>Choose “弹性IP”， click “分配新地址”</p>
<p><img src="resources/56A9928FEBB1D574AED9D96CCFAF11D6.jpg" alt="IMAGE" width="1335" height="277"></p>
<p>Choose the created ip, and click “操作”， choose “关联地址”<br>
<img src="resources/5E36CF29BDE1083DA520937B3DEF9835.jpg" alt="IMAGE" width="1949" height="197"></p>
<p>In next page, choose vAER instance and click “关联”<br>
<img src="resources/E7C03A19EDA289AED8D19879C10B9F69.jpg" alt="IMAGE" width="1299" height="486"></p>
<h3 id="disable-check-sourcedestination-ip">1.2.4 Disable check “Source/destination IP”</h3>
<p>Choose the vaer instance, click the “操作”, choose “联网”, click “更改源/目标。检查”<br>
<img src="resources/87A59A4AA7AB31CF783EAC8625ABE591.jpg" alt="IMAGE" width="1752" height="395"><br>
In the next page, click “是，请禁用”<br>
<img src="resources/EDCA1E4A35218DB025E4C51A38950F53.jpg" alt="IMAGE" width="486" height="306"></p>
<h3 id="contact-axesdn-to-configure-the-vaer">1.2.4 Contact AXESDN to configure the vAER</h3>
<p>Give below information to O&amp;M team:</p>
<ul>
<li>vAER floating IP address</li>
</ul>
<h3 id="route-the-traffic-to-vaer">1.2.5 Route the traffic to vAER</h3>
<p>On “VPC控制面板”, Choose “路由表”, choose the VPC’s route table, on the left table, choose “路由”，click “编辑路由”<br>
<img src="resources/AC8F0816CE5B381AF02CCC2230D97DC0.jpg" alt="IMAGE" width="2350" height="600"></p>
<p>Add all your NAAS remote routes into the table, and point to vAER instance. And click “保存路由”<br>
<img src="resources/0B29AA4CB03F848D1B636AFD9D281E0A.jpg" alt="IMAGE" width="1292" height="396"></p>
<h3 id="permit-remote-sites-internal-network-traffic">1.2.7 Permit remote sites’ internal network traffic</h3>
<p>For the VMs inside the VPC which will use vaer to forward the traffic, need to permit the traffic:</p>
<ul>
<li>traffic from vaer</li>
<li>traffic from remote sites’ network</li>
</ul>
<p>In this case, we need to permit 172.31.16.0/20 and 10.23.2.0/24 all traffic or some traffic depends on customer’s requirment.</p>
<p>Open the “安全组” of your instance and edit the security group as below:<br>
<img src="resources/66E7C19F3DF956D464A5C2BA91DC05A8.jpg" alt="IMAGE" width="1126" height="387"></p>
<h3 id="chek-the-connectivity">1.2.8 Chek the connectivity</h3>
<p>Login to one of the Instances in the same VPC, use</p>
<ol>
<li>ping to check the connectivity</li>
<li>Traceroute to check the route path.</li>
<li>iperf or run tool ant to generate the traffic report,including:</li>
</ol>
<ul>
<li>connectivity</li>
<li>latency</li>
<li>packet loss</li>
<li>mtu/mss</li>
<li>tcp bandwidth</li>
<li>udp bandwidth</li>
</ul>
<h2 id="multiple-subnets-in-one-vpc">1.3 Multiple subnets in one VPC</h2>
<p><img src="resources/A9F7263A5638F0BB1AC08AF83BFCD588.jpg" alt="IMAGE" width="947" height="251"></p>
<h3 id="create-a-new-subnets-in-the-vpc">1.3.1 Create a new subnets in the VPC</h3>
<p>Check “Appendix- 1. Create VPC/Routes” about “create subnets part”.</p>
<h3 id="ensure-the-ec2-instance-in-new-subnets-has-the-correct-security-group-configured">1.3.2 Ensure the EC2 instance in new subnets has the correct security group configured</h3>
<p>Refer to: “1.2.7 Permit remote sites’ internal network traffic”</p>
<h3 id="contact-om-team-to-add-the-new-routes-in-vaer">1.3.3 Contact O&amp;M team to add the new routes in vaer</h3>
<p>The new subnets’ network need to add to vaer for route forwarding.</p>
<h3 id="add-the-new-subnets-in-the-same-route-table-of-subnet-1">1.3.4 Add the new subnets in the same route table of subnet-1</h3>
<p>Choose “路由表” in the left panel,<br>
<img src="resources/E866E3516D0D2CA9A4BE26C38444CB4B.jpg" alt="IMAGE" width="203" height="182"><br>
choose the route table, go to“子网关联”， choose the subnets need to add the route table which has vaer routes configured,<br>
<img src="resources/9E8B8EECE128433B0FF94EFF7DFEBDE0.jpg" alt="IMAGE" width="1343" height="562"><br>
Click save. Check there’s two subnets connected to the route table:<br>
<img src="resources/B27CFA9C9A53E2B6019DA43A75D4D56C.jpg" alt="IMAGE" width="965" height="338"></p>
<h3 id="check-the-connectivity">1.3.5 Check the connectivity</h3>
<p>Refer to “1.2.8 Chek the connectivity” for details.</p>
<p>#2. ANOS L7</p>
<h2 id="basic-info-1">2.1 Basic Info</h2>
<h3 id="topology-1">2.1.1 Topology</h3>
<p><img src="resources/086475AB05598EEC653B4E27E500DC96.jpg" alt="IMAGE" width="763" height="315"></p>
<h3 id="scenarios-support-1">2.1.2 Scenarios Support</h3>

<table>
<thead>
<tr>
<th>ANOS Type</th>
<th>AWS Support Status</th>
</tr>
</thead>
<tbody>
<tr>
<td>ANOS L7 in VPC level*</td>
<td>Yes</td>
</tr>
<tr>
<td>ANOS L7 in VM level**</td>
<td>Yes</td>
</tr>
</tbody>
</table><blockquote>
<p>* In VPC level means the configuration in VPC, and all the VMs in the VPC can get benefit of it.<br>
** In VM level means customer need to do the configuration in each VMs.</p>
</blockquote>
<h2 id="configuration-1">2.2 Configuration</h2>
<h3 id="prepare-a-axesdn-dns-isp">2.2.1 Prepare a AXESDN DNS ISP</h3>
<p>For example, we have a DNS server named: 218.98.18.43</p>
<h3 id="create-dhcp-options-in-aws">2.2.2 Create DHCP Options in AWS</h3>
<p><strong>VPC --&gt; DHCP选项集 --&gt; 创建DHCP选项集</strong></p>
<p><img src="resources/AB0499BB1B196EF1C1FBB1AB522D24B0.jpg" alt="IMAGE" width="1289" height="660"></p>
<p><strong>Result</strong><br>
<img src="resources/822E39CFA9F007FEBAFB631654E7CE81.jpg" alt="IMAGE" width="1078" height="215"></p>
<h3 id="modify-vpc-dhcp-options-set-to-the-new-created-dhcp">2.2.3 Modify VPC DHCP options set to the new created DHCP</h3>
<p><strong>Choose the VPC in VPC list and choose "Edit DHCP option set"</strong><br>
<img src="resources/1EE29618BE9059C6EBDFE11583D078CF.jpg" alt="IMAGE" width="1601" height="332"></p>
<p><strong>Modify the DHCP options to the one we just created</strong><br>
<img src="resources/54D026F7525E5ED444FDB547B89D892F.jpg" alt="IMAGE" width="788" height="270"></p>
<h3 id="test-create-instance-and-check-dns-server">2.2.4 Test-Create Instance and check DNS server</h3>
<blockquote>
<p>For already created instances, to make new DNS config work, need a reboot or wait for current DHCP infomation expired.</p>
</blockquote>
<p><strong>Check DNS in ubuntu:</strong><br>
<img src="resources/E8A8814C2DF41D74B5C4CD66B8BB90C0.jpg" alt="IMAGE" width="404" height="108"></p>
<p>It will show 127.0.0.53, but actually it use our 218.98.18.43 as dns server if you use command to check real DNS in ubuntu:<br>
<code>systemd-resolve --status | grep 'DNS Servers'</code></p>
<p>eg.</p>
<pre><code>ubuntu@VM-1-9-ubuntu:~$ systemd-resolve --status | grep 'DNS Servers'  
DNS Servers: 218.98.18.43  
</code></pre>
<h1 id="appendix--1.-create-vpcroutes">Appendix- 1. Create VPC/Routes</h1>
<p>This part decribes the steps to prepare a VPC on AWS.</p>
<h3 id="step-1-create-vpc">Step 1) Create VPC</h3>
<p><img src="resources/6C9F5D0EBAE089C9EFBD8DA640CEBBD1.jpg" alt="IMAGE" width="1054" height="401"></p>
<p><img src="resources/CC8087BE63860B054F3862F12914A9C3.jpg" alt="IMAGE" width="988" height="269"></p>
<h3 id="step-2-create-subnets">Step 2) Create subnets</h3>
<p><img src="resources/E743B0B5E7CFB8C0B7CA5A6693DB906A.jpg" alt="IMAGE" width="1057" height="530"><br>
<img src="resources/B7346EA90325F17FF58F7192F6B8D951.jpg" alt="IMAGE" width="1072" height="275"></p>
<h3 id="step-3-create-routes">Step 3) Create routes</h3>
<p><img src="resources/7D52FAC5C5C1EC68953211F0C388F663.jpg" alt="IMAGE" width="1067" height="308"><br>
<img src="resources/F375B0629DB6F505B7D0D067021B79B2.jpg" alt="IMAGE" width="1045" height="288"></p>
<p><strong>Set it to the main routes for VPC</strong></p>
<p>Choose the just created route table, click “操作”， choose“设置主路由表”，</p>
<p><img src="resources/21ED101AA8B5B00362A46649F5A40E06.jpg" alt="IMAGE" width="895" height="426"></p>
<h3 id="step-4-create-internet-gateway（igw）">Step 4) Create internet gateway（igw）</h3>
<p><img src="resources/5E267CEE40D24AACED44289D8459EE12.jpg" alt="IMAGE" width="1062" height="241"><br>
Click “创建”<br>
<img src="resources/1731EF446230783E1EF58E2A7BDB757D.jpg" alt="IMAGE" width="981" height="265"></p>
<p>Attach the new created igw to VPC<br>
<img src="resources/5913BE894A7D82CF6DEF2080936568B0.jpg" alt="IMAGE" width="1003" height="347"><br>
Click “附加到VPC”<br>
<img src="resources/33CA64350E8BFB55AF3B31829626C4C5.jpg" alt="IMAGE" width="1293" height="309"><br>
Click “附加”</p>
<h3 id="step-5-use-igw-in-vpc-route">Step 5) Use IGW in VPC route</h3>
<p>In the “VPC控制面板”, choose “路由表”， get the routes list, choose the route table called: “axesdn_naas_test_route_table”<br>
<img src="resources/88A7AF2715D40F822CDD6A0911CCAFF6.jpg" alt="IMAGE" width="1424" height="510"></p>
<p>Click “编辑路由” on the right-bottom panel:<br>
<img src="resources/00F2213BFD05F2337BB4FF65DCE5F64B.jpg" alt="IMAGE" width="1323" height="368"></p>
<p>Click “保存路由”<br>
<img src="resources/E958BD28C9FA405050BB40A37B605813.jpg" alt="IMAGE" width="978" height="218"></p>
<h1 id="appendix-2.-share-image-to-customer-target-om-team">Appendix-2. Share image to customer (Target: O&amp;M team)</h1>
<p>Choose “AMI” on EC2 page, click the AMI, choose action, and choose “Modify Image Permissions”<br>
<img src="resources/6DAD7FBCF108D043EF489B570A9E0257.jpg" alt="IMAGE" width="1920" height="979"></p>
<p>Input customer’s aws account id, and click save. Keep it in “Private” status<br>
<img src="resources/F2239C4173899909C5270ED99FB11FD1.jpg" alt="IMAGE" width="559" height="296"></p>

    </div>
  </div>
</body>

</html>
