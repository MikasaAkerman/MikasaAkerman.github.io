- 方案一

```bash
#!/usr/bin/env bash

git_dir=".git"

read_dir(){
    if [ -e "$1/$git_dir" ]
    then
      echo "pull $1"

      old=$(pwd)
      cd "$1" || return

      pull "$1"

      cd "$old" || return
    else
      # shellcheck disable=SC2045
      for element in $(ls "$1")
      do
          if [ -d "$1/$element" ] && [[ $element != "pb" ]]
          then
              read_dir "$1/$element"
          fi
      done
    fi
}

pull(){
  pull_result=$(git pull)

  if [[ $pull_result != "Already up to date." && $1 =~ "proto" && -e "gen_pb.sh" ]]
  then
    sh gen_pb.sh
  fi
}

read_dir .
```

- 方案二

```she
PRJ_CAAS_PATH=/C/Workspaces/src/project
ARRAY_PROJECT_NAME=("baseutil" "basic_proto" "platformutil" "present_proto")

PRJ_CAAS_PATH_basicservice=/C/Workspaces/src/project/basic_service
ARRAY_PROJECT_NAME_basicservice=("service1" "service2")

PRJ_CAAS_PATH_common=/C/Workspaces/src/project/common
ARRAY_PROJECT_NAME_common=("utils")

PRJ_CAAS_PATH_present_service=/C/Workspaces/src/project/present_service
ARRAY_PROJECT_NAME_present_service=("service3" "service4")

function gitall() {
                if [ $# -lt 2 ]; then
                                echo "Param Error"
                fi
                
                cd ${PRJ_CAAS_PATH}
                for prj_nm in ${ARRAY_PROJECT_NAME[@]}
                do
                                cd $prj_nm
                                echo `tput setaf 3;pwd;tput setaf 6;`"("`git rev-parse --abbrev-ref HEAD`")"
                                tput sgr0
                                $*
                                cd ..
                done
				
				
				cd ${PRJ_CAAS_PATH_basicservice}
                for prj_nm in ${ARRAY_PROJECT_NAME_basicservice[@]}
                do
                                cd $prj_nm
                                echo `tput setaf 3;pwd;tput setaf 6;`"("`git rev-parse --abbrev-ref HEAD`")"
                                tput sgr0
                                $*
                                cd ..
                done
                
				
				cd ${PRJ_CAAS_PATH_common}
                for prj_nm in ${ARRAY_PROJECT_NAME_common[@]}
                do
                                cd $prj_nm
                                echo `tput setaf 3;pwd;tput setaf 6;`"("`git rev-parse --abbrev-ref HEAD`")"
                                tput sgr0
                                $*
                                cd ..
                done
				
				
				cd ${PRJ_CAAS_PATH_present_service}
                for prj_nm in ${ARRAY_PROJECT_NAME_present_service[@]}
                do
                                cd $prj_nm
                                echo `tput setaf 3;pwd;tput setaf 6;`"("`git rev-parse --abbrev-ref HEAD`")"
                                tput sgr0
                                $*
                                cd ..
                done
}
```

