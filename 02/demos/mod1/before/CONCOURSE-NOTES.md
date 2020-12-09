### FLY CLI COMMANDS

    <b>fly targets</b> 
    	shows the concourse deployments
    		name  url                    team  expiry                       
    		ps    http://localhost:8080  main  Thu, 10 Dec 2020 01:33:27 UTC
    
    <b>fly -t <target> user info</b>
    	shows the user role you have
    	  ./fly -t ps userinfo
    	  username  team/role 
          test      main/owner
    
    <b>fly -t <target> pipelines</b>
    	shows pipelines running for target
    	  ./fly -t ps pipelines
    	  name  paused  public  last updated
     	
    <b>fly -t <target> set-pipeline -c <yml file> -p <pipeline name></b>
    	run a pipeline * initially it will be in a paused state
    	  ./fly -t ps set-pipeline -c /Users/ryanschulte/Documents/concourse/Training/.  	Pluralsight/concourse-getting-started/02/demos/mod1/before/mod1-pipeline.yml -p 	mod1-pipeline 
    		it will show you the yml file contents then ask you y/n if you want to ruin it
    		then it will provide the url for the pipeline
	
  
### BASIC INFO

	<b>TASK</b>
		part of a job that does something specific 
		1. Similar to a function, with input and output
		2. Smallest configurable unit
		3. Runs in containers using designed image 
			- container is destroyed upon completion
		4. Specify a statement to execute
        5. resources are the ONLY durable storage mechanism in a pipeline, everything else is stateless
        6. Task inputs produce dirs full of artifacts within the container
        7. Task outputs are stored on worker filesystem and mounted into containers for subsequent tasks within
            the same job, DOES NOT SHARE JOBS
        8. You can test tasks WITHOUT testing the pipeline i.e unit testing
		
		EXAMPLE TASK:
			——-
			platform: linux                        # which platform the task needs to be run on

            image_resource:                        # Sets specific container image that will run the task
			    type: docker-image
                source: {repository: unbuntu}      # "repository" is required source param and optional ones 
                                                   # could be tag or username/password, can also use your own
                                                   # docker image if you want pre-baked things
            inputs:
                name: demo-folder                  # Specify artifacts availabler in current dir @ task run time    
                                                   # An oiptional is path name, inputs can come from cli params
                                                   # a "get" step within build plabn or from "outputs" of 
                                                   # previous "task"
            
            outputs:                               # Artifacts produced by the task, a dir available to later
            - name: compiled-app                   # steps in the build plan * another task would reference
                                                   # "comp;iled-app" as a reference 

            run:                                   # Represents the command to run in the container, want to 
                path: find                         # make sure this is small, "path" could point a specific
                args: [.]                          # bash command or script provided by "input"

        

### UI INFO

	- a dotted line in the UI means the job needs to be triggered manually