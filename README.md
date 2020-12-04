# -Priority-based-Dynamic-Resource-Mapping-algorithm-for-load-balancing-in-cloud

//Dynamic Priority Algorithm





package dynamic;


import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.LinkedList;
import java.util.List;





import org.cloudbus.cloudsim.Cloudlet;
import org.cloudbus.cloudsim.CloudletSchedulerTimeShared;
import org.cloudbus.cloudsim.CloudletSchedulerSpaceShared;
import org.cloudbus.cloudsim.Datacenter;
import org.cloudbus.cloudsim.DatacenterBroker;
import org.cloudbus.cloudsim.DatacenterCharacteristics;
import org.cloudbus.cloudsim.Host;
import org.cloudbus.cloudsim.Log;
import org.cloudbus.cloudsim.Pe;
import org.cloudbus.cloudsim.Storage;
import org.cloudbus.cloudsim.UtilizationModel;
import org.cloudbus.cloudsim.UtilizationModelFull;
import org.cloudbus.cloudsim.Vm;
import org.cloudbus.cloudsim.VmAllocationPolicySimple;
import org.cloudbus.cloudsim.VmSchedulerTimeShared;
import org.cloudbus.cloudsim.core.CloudSim;
import org.cloudbus.cloudsim.provisioners.BwProvisionerSimple;
import org.cloudbus.cloudsim.provisioners.PeProvisionerSimple;
import org.cloudbus.cloudsim.provisioners.RamProvisionerSimple;


/**
 * To create
 * a datacenter with FIVE virtual machines and run FIFTY
 * cloudlets on it. The cloudlets run in
 * VMs with the same MIPS requirements.
 * The cloudlets will take the same time to
 * complete the execution.
 */
public class dynamic {

	/** The cloudlet list. */
	private static List<Cloudlet> cloudletList;

	/** The vmlist. */
	private static List<Vm> vmlist;

	/**
	 * Creates main() to run this example
	 */
	public static void main(String[] args) {

		Log.printLine("Initializing the cloudsim package");

	        try {
	        	// First step: Initialize the CloudSim package. It should be called
	            	// before creating any entities.
	            	int num_user = 1;   // number of cloud users
	            	Calendar calendar = Calendar.getInstance();
	            	boolean trace_flag = false;  // mean trace events

	            	// Initialize the CloudSim library
	            	CloudSim.init(num_user, calendar, trace_flag);

	            	// Second step: Create Datacenters
	            	//Datacenters are the resource providers in CloudSim. We need at list one of them to run a CloudSim simulation
	            	@SuppressWarnings("unused")
					Datacenter datacenter0 = createDatacenter("Datacenter_0");

	            	//Third step: Create Broker
	            	DatacenterBroker broker = createBroker();
	            	int brokerId = broker.getId();

	            	// Algorithm Step-1: Select VM1, VM2…, VMn number of virtual machines and C1, C2…, Cm number of cloudlets 
	            	vmlist = new ArrayList<Vm>();
	            		int X;
                        int vms = 5;
                        int cloudlets = 50;
            // Algorithm Step-3: Select a value X: If cloudlets are greater than virtual machine then using equation no. 1 else use equation no.2
                        if(cloudlets > vms) {
                        	X=(cloudlets/vms);			// equation 1
                        }else {
                        	X=(vms/cloudlets);			// equation 2
                        }
                        
                        
                        
                        
                        
                        
	            	//VM description
	            	int vmid = 0;
	            	long size = 1000; //image size (MB)
                        int ram = 512; //vm memory (MB)
                        int mips = 450;
                        long bw = 1000;
                        int pesNumber = 1; //number of cpus
                        String vmm = "Xen"; //VMM name


	            	//create two VMs
                        int jk=1;
                        
                        Vm vm[]=new Vm[vms];
	            	
                        for(int i=0; i<vms;i++)
                        {
                            if (i%2==0 )
				mips += jk;
			else 
				mips -= jk;
                            vm[i] = new Vm(vmid, brokerId, mips, pesNumber, ram, bw, size, vmm, new CloudletSchedulerSpaceShared());
                            vmid++;
	            	jk+=200;
	            	vmlist.add(vm[i]);
	            	
                        }
                        vmid--;
	            	
                        
        // Algorithm Step-2: Sort in ascending order the virtual machines and cloudlets according to MIPS and million instructions respectively. 
                      
                        
                        List<Vm> lstvms = vmlist;
        for (int a = 0; a < lstvms.size(); a++) {
            for (int b = a + 1; b < lstvms.size(); b++) {
                if (lstvms.get(a).getMips() > lstvms.get(b).getMips()) {
                    Vm temp = lstvms.get(a);
                    lstvms.set(a, lstvms.get(b));
                    lstvms.set(b, temp);
                }
            }
        }
                         for (Vm mm : lstvms) {
            System.out.println("Vm id = " + mm.getId() + " - MIPS = " + mm.getMips());
        }
                        
                        
                        //add the VMs to the vmList
	            	vmlist.add(vm[1]);
                        

	            	//submit vm list to the broker
	            	broker.submitVmList(lstvms);


	            	//Fifth step: Create two Cloudlets
	            	cloudletList = new ArrayList<Cloudlet>();

                        
                                               
	            	//Cloudlet properties
	            	int id = 0;
	            	long length = 4000;
		long fileSize = 300;
		long outputSize = 300;
		
	            	UtilizationModel utilizationModel = new UtilizationModelFull();

                        Cloudlet[] cloudlet = new Cloudlet[cloudlets];
                        
                        for(int i=0;i<cloudlets;i++){
			if (i%2==0 || i<2)
				length +=  6500;
			else 
				length -=  3277;
			
			cloudlet[i] = new Cloudlet(++id, length, pesNumber, fileSize, outputSize, utilizationModel, utilizationModel, utilizationModel);
			// setting the owner of these Cloudlets
			cloudlet[i].setUserId(brokerId);
			cloudletList.add(cloudlet[i]);
		}
                        
                   
                        
                        //add code here
                      
 

                        
                        
                        
                        
                        
                        
                        
                        
                        
                         List<Cloudlet> lstCloudlets = cloudletList;
        for (int a = 0; a < lstCloudlets.size(); a++) {
            for (int b = a + 1; b < lstCloudlets.size(); b++) {
                if (lstCloudlets.get(a).getCloudletLength() > lstCloudlets.get(b).getCloudletLength()) {
                    Cloudlet temp = lstCloudlets.get(a);
                    lstCloudlets.set(a, lstCloudlets.get(b));
                    lstCloudlets.set(b, temp); // sorting to cloudlets in ascending order
                }
            }
        }
        
     
        // NEED TO ADD FIVE FOR LOOPS AS WELL AS NEED TO SUBMIT 5 TIMES 
        // Step-4: Divide the sorted cloudlets into groups where each group must have X number of cloudlets
        
      
            // create five empty lists 
            List<Cloudlet> first = new ArrayList<Cloudlet>(); 
            List<Cloudlet> second = new ArrayList<Cloudlet>(); 
            List<Cloudlet> third = new ArrayList<Cloudlet>(); 
            List<Cloudlet> fourth = new ArrayList<Cloudlet>(); 
            List<Cloudlet> fifth = new ArrayList<Cloudlet>();
      
            // get size of the list 
          
      
            // First size)/2 element copy into list 
            // first and rest second list 
            for (int i = 0; i< 10; i++) 
                first.add(lstCloudlets.get(i)); 
            	
            // Second size)/2 element copy into list 
            // first and rest second list 
            for (int i = 10; i < 20; i++) 
                second.add(lstCloudlets.get(i)); 
            
            for (int i = 20; i < 30; i++) 
                third.add(lstCloudlets.get(i)); 
            
            for (int i = 30; i < 40; i++) 
                fourth.add(lstCloudlets.get(i)); 
            
        for (int i = 40; i < 50; i++) 
                fifth.add(lstCloudlets.get(i)); 

            List<Cloudlet> First = first;
            for (int a = 0; a < first.size(); a++) {
                for (int b = a + 1; b < first.size(); b++) {
                    if (first.get(a).getCloudletLength() < first.get(b).getCloudletLength()) {
                        Cloudlet temp = first.get(a);
                        first.set(a, first.get(b));
                        first.set(b, temp); }}}
           
            List<Cloudlet> Second = second;
            for (int a = 0; a < second.size(); a++) {
                for (int b = a + 1; b < second.size(); b++) {
                    if (second.get(a).getCloudletLength() < second.get(b).getCloudletLength()) {
                        Cloudlet temp = second.get(a);
                        second.set(a, second.get(b));
                        second.set(b, temp); }}}
           
            List<Cloudlet> Third = third;
            for (int a = 0; a < third.size(); a++) {
                for (int b = a + 1; b < first.size(); b++) {
                    if (third.get(a).getCloudletLength() < third.get(b).getCloudletLength()) {
                        Cloudlet temp = third.get(a);
                        third.set(a, third.get(b));
                        third.set(b, temp); }}}
           
         List<Cloudlet> Fourth = fourth;
            for (int a = 0; a < fourth.size(); a++) {
                for (int b = a + 1; b < fourth.size(); b++) {
                    if (fourth.get(a).getCloudletLength() < fourth.get(b).getCloudletLength()) {
                        Cloudlet temp = fourth.get(a);
                        fourth.set(a, fourth.get(b));
                        fourth.set(b, temp); }}}
           
            List<Cloudlet> Fifth= fifth;
            for (int a = 0; a < fifth.size(); a++) {
                for (int b = a + 1; b < fifth.size(); b++) {
                    if (fifth.get(a).getCloudletLength() < fifth.get(b).getCloudletLength()) {
                        Cloudlet temp = fifth.get(a);
                        fifth.set(a, fifth.get(b));
                        fifth.set(b, temp); }}}
           

            for (Cloudlet cl : First) {
System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
}
            for (Cloudlet cl : Second) {
            	System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
            	}
            for (Cloudlet cl : Third) {
            	System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
            	}
            for (Cloudlet cl : Fourth) {
            	System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
            	}
           for (Cloudlet cl : Fifth) {
            	System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
            	}
                         for (Cloudlet cl : lstCloudlets) {
            System.out.println("Cloudlet id = " + cl.getCloudletId() + " - Length = " + cl.getCloudletLength());
        }
                         

                        
     	            	
     	            	
                        //submit cloudlet lists to the broker
	            //	broker.submitCloudletList(lstCloudlets);
	            	//broker.submitCloudletList(first);
	            //	broker.submitCloudletList(second);
	            	//broker.submitCloudletList(third);
	           // 	broker.submitCloudletList(fourth);
	            //	broker.submitCloudletList(fifth);
	               	broker.submitCloudletList(First);
	               	broker.submitCloudletList(Second);
	               	broker.submitCloudletList(Third);
	               	broker.submitCloudletList(Fourth);
	              	broker.submitCloudletList(Fifth);
	            	
	            	// Step 5: Step-5: Map each group sequentially to the sorted virtual machines. 
	            	for (Cloudlet cl : First) 
 	            		broker.bindCloudletToVm(cl.getCloudletId(),vm[3].getId());
 	            	
 	            	for (Cloudlet cl : Second) 
 	            		broker.bindCloudletToVm(cl.getCloudletId(),vm[1].getId());
 	            	
 	            	for (Cloudlet cl : Third) 
 	            		broker.bindCloudletToVm(cl.getCloudletId(),vm[0].getId());
 	            	
 	            	for (Cloudlet cl : Fourth) 
 	            		broker.bindCloudletToVm(cl.getCloudletId(),vm[2].getId());
 	            	
 	            	for (Cloudlet cl : Fifth) 
 	            		broker.bindCloudletToVm(cl.getCloudletId(),vm[4].getId());
	        
	            

	            	// Sixth step: Starts the simulation
	            	CloudSim.startSimulation();


	            	// Final step: Print results when simulation is over
	            	List<Cloudlet> newList = broker.getCloudletReceivedList();

	            	CloudSim.stopSimulation();

	            	printCloudletList(newList);

	            	Log.printLine("Simulation Completed");
	        }
	        catch (Exception e) {
	            e.printStackTrace();
	            Log.printLine("The simulation has been terminated due to an unexpected error");
	        }
	    }
	
	
         	
	
		private static Datacenter createDatacenter(String name){

	        // Here are the steps needed to create a PowerDatacenter:
	        // 1. We need to create a list to store
	    	//    our machine
	    	List<Host> hostList = new ArrayList<Host>();

	        // 2. A Machine contains one or more PEs or CPUs/Cores.
	    	// In this example, it will have only one core.
	    	List<Pe> peList = new ArrayList<Pe>();

	    	int mips = 302400;

	        // 3. Create PEs and add these into a list.
	    	peList.add(new Pe(0, new PeProvisionerSimple(mips))); // need to store Pe id and MIPS Rating

	        //4. Create Host with its id and list of PEs and add them to the list of machines
	        int hostId=0;
		int ram = 102400; //host memory (MB)
		long storage = 1000000; //host storage
		int bw = 200000;

	        hostList.add(
	    			new Host(
	    				hostId,
	    				new RamProvisionerSimple(ram),
	    				new BwProvisionerSimple(bw),
	    				storage,
	    				peList,
	    				new VmSchedulerTimeShared(peList)
	    			)
	    		); // This is our machine


	        // 5. Create a DatacenterCharacteristics object that stores the
	        //    properties of a data center: architecture, OS, list of
	        //    Machines, allocation policy: time- or space-shared, time zone
	        //    and its price (G$/Pe time unit).
	        String arch = "x86";      // system architecture
	        String os = "Linux";          // operating system
	        String vmm = "Xen";
	        double time_zone = 10.0;         // time zone this resource located
	        double cost = 3.0;              // the cost of using processing in this resource
	        double costPerMem = 0.05;		// the cost of using memory in this resource
	        double costPerStorage = 0.001;	// the cost of using storage in this resource
	        double costPerBw = 0.0;			// the cost of using bw in this resource
	        LinkedList<Storage> storageList = new LinkedList<Storage>();	//we are not adding SAN devices by now

	        DatacenterCharacteristics characteristics = new DatacenterCharacteristics(
	                arch, os, vmm, hostList, time_zone, cost, costPerMem, costPerStorage, costPerBw);


	        // 6. Finally, we need to create a PowerDatacenter object.
	        Datacenter datacenter = null;
	        try {
	            datacenter = new Datacenter(name, characteristics, new VmAllocationPolicySimple(hostList), storageList, 0);
	        } catch (Exception e) {
	            e.printStackTrace();
	        }

	        return datacenter;
	    }

	    // Creation of Broker
		
	    private static DatacenterBroker createBroker(){

	    	DatacenterBroker broker = null;
	        try {
			broker = new DatacenterBroker("Broker");
		} catch (Exception e) {
			e.printStackTrace();
			return null;
		}
	    	return broker;
	    }

	    /**
	     * Prints the Cloudlet objects
	     * @param list  list of Cloudlets
	     */
	    private static void printCloudletList(List<Cloudlet> list) {
	        int size = list.size();
	        Cloudlet cloudlet;

	        String indent = "    ";
	        Log.printLine();
	        Log.printLine("========== OUTPUT ==========");
	        Log.printLine("Cloudlet ID" + indent + "STATUS" + indent +
	                "Data center ID" + indent + "VM ID" + indent + "Time" + indent + "Start Time" + indent + "Finish Time");

	        DecimalFormat dft = new DecimalFormat("###.##");
	        for (int i = 0; i < size; i++) {
	            cloudlet = list.get(i);
	            Log.print(indent + cloudlet.getCloudletId() + indent + indent);

	            if (cloudlet.getCloudletStatus() == Cloudlet.SUCCESS){
	                Log.print("SUCCESS");

	            	Log.printLine( indent + indent + cloudlet.getResourceId() + indent + indent + indent + cloudlet.getVmId() +
	                     indent + indent + dft.format(cloudlet.getActualCPUTime()) + indent + indent + dft.format(cloudlet.getExecStartTime())+
                             indent + indent + dft.format(cloudlet.getFinishTime()));
	            }
                    else
                    {
                        Log.print("Failure");
                    }
	        }

	    }
}
