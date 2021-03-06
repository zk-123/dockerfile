FROM frolvlad/alpine-glibc:alpine-3.9

ENV JAVA_TAR_GZ_NAME=jdk-7u80-linux-x64.tar.gz \
    JAVA_DIR_NAME=jdk1.7.0_80 \
    JAVA_HOME=/etc/jdk \
    TOMCAT_TAR_GZ_NAME=apache-tomcat-7.0.85.tar.gz \
    TOMCAT_DIR_NAME=apache-tomcat-7.0.85 \
    CATALINA_HOME=/opt/tomcat
    

WORKDIR ./

RUN cd /tmp && \
    echo "https://mirrors.ustc.edu.cn/alpine/v3.9/main" > /etc/apk/repositories && \
    echo "https://mirrors.ustc.edu.cn/alpine/v3.9/community" >> /etc/apk/repositories  && \
    apk add --no-cache bash tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "Asia/Shanghai" > /etc/timezone && \
    wget http://192.168.24.109/${TOMCAT_TAR_GZ_NAME} && \
    wget http://192.168.24.109/${JAVA_TAR_GZ_NAME} && \
    tar -xvf ${TOMCAT_TAR_GZ_NAME} && \
    tar -xvf ${JAVA_TAR_GZ_NAME} && \
    mkdir -p ${JAVA_HOME} && \
    mkdir -p ${CATALINA_HOME} && \
    mv "/tmp/${JAVA_DIR_NAME}/"* "${JAVA_HOME}" && \
    mv "/tmp/${TOMCAT_DIR_NAME}/"* "${CATALINA_HOME}" && \
    rm -rf "$JAVA_HOME/lib/missioncontrol" \
           "$JAVA_HOME/lib/visualvm" \
           "$JAVA_HOME/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/plugin.jar" \
           "$JAVA_HOME/jre/lib/ext/jfxrt.jar" \
           "$JAVA_HOME/jre/bin/javaws" \
           "$JAVA_HOME/jre/lib/javaws.jar" \
           "$JAVA_HOME/jre/lib/desktop" \
           "$JAVA_HOME/jre/plugin" \
           "$JAVA_HOME/jre/lib/"deploy* \
           "$JAVA_HOME/jre/lib/"*javafx* \
           "$JAVA_HOME/jre/lib/"*jfx* \
           "$JAVA_HOME/jre/lib/amd64/libdecora_sse.so" \
           "$JAVA_HOME/jre/lib/amd64/"libprism_*.so \
           "$JAVA_HOME/jre/lib/amd64/libfxplugins.so" \
           "$JAVA_HOME/jre/lib/amd64/libglass.so" \
           "$JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so" \
           "$JAVA_HOME/jre/lib/amd64/"libjavafx*.so \
           "$JAVA_HOME/jre/lib/amd64/"libjfx*.so && \
    rm -rf "$JAVA_HOME/jre/bin/jjs" \
           "$JAVA_HOME/jre/bin/keytool" \
           "$JAVA_HOME/jre/bin/orbd" \
           "$JAVA_HOME/jre/bin/pack200" \
           "$JAVA_HOME/jre/bin/policytool" \
           "$JAVA_HOME/jre/bin/rmid" \
           "$JAVA_HOME/jre/bin/rmiregistry" \
           "$JAVA_HOME/jre/bin/servertool" \
           "$JAVA_HOME/jre/bin/tnameserv" \
           "$JAVA_HOME/jre/bin/unpack200" \
           "$JAVA_HOME/jre/lib/ext/nashorn.jar" \
           "$JAVA_HOME/jre/lib/jfr.jar" \
           "$JAVA_HOME/jre/lib/jfr" \
           "$JAVA_HOME/jre/lib/oblique-fonts" && \
    rm -rf "/tmp/"* && \
    echo "export JAVA_HOME=$JAVA_HOME" >> ~/.bashrc && \
    echo "export PATH=$CATALINA_HOME/bin:$JAVA_HOME/bin:$PATH" >> ~/.bashrc && \
    echo "export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar" >> ~/.bashrc && \
    echo "export CATALINA_HOME=$CATALINA_HOME" >> ~/.bashrc && \
    echo "export TOMCAT_HOME=$CATALINA_HOME" >> ~/.bashrc && \
    echo "PS1=\"[\u@\h \W]# \"" >>  ~/.bashrc && \
    echo "cd $CATALINA_HOME" >>  ~/.bashrc && \
    mkdir /docker-tomcat-init.d && \
    touch /init.sh && \
    chmod +x /init.sh && \
    echo "#!/bin/bash" >> /init.sh && \
    echo -e "if [[ \`ls -A /docker-tomcat-init.d|wc -w\` -eq 0 ]] ;then\n  $CATALINA_HOME/bin/catalina.sh run\n  exit 0 \nfi;" >> /init.sh && \
    echo "mkdir ${CATALINA_HOME}/webapps/"'${PROJECT_NAME}'"" >> /init.sh && \
    echo "unzip /docker-tomcat-init.d/war/"'${PROJECT_NAME}'".war -d ${CATALINA_HOME}/webapps/"'${PROJECT_NAME}'"" >> /init.sh && \
    echo "cp -r /docker-tomcat-init.d/conf/* -d ${CATALINA_HOME}/webapps/"'${PROJECT_NAME}'"/WEB-INF/classes" >> /init.sh && \
    echo "cp -r /docker-tomcat-init.d/webapps -d ${CATALINA_HOME}/" >> /init.sh && \
    echo "rm -rf ${CATALINA_HOME}/logs" >> /init.sh && \
    echo "ln -s /docker-tomcat-init.d/logs ${CATALINA_HOME}/logs" >> /init.sh && \
    echo "$CATALINA_HOME/bin/catalina.sh run" >> /init.sh
    
EXPOSE 8080
ENTRYPOINT ["/bin/sh", "-c", "/init.sh"]

