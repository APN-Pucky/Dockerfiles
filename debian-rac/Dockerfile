FROM apnpucky/debian-base as base
LABEL maintainer="APN-Pucky"
ADD https://raw.githubusercontent.com/APN-Pucky/rolling-ansible-container/master/bootstrap-rac.sh /bootstrap-rac.sh
RUN chmod +x /bootstrap-rac.sh && /bootstrap-rac.sh && rm /bootstrap-rac.sh 
COPY ansible /ansible/
RUN cd ansible && ansible localhost --connection=local  --module-name include_role --args name=base && cd .. && rm -r /ansible 

FROM  apnpucky/debian-dev as dev
LABEL maintainer="APN-Pucky"
ADD https://raw.githubusercontent.com/APN-Pucky/rolling-ansible-container/master/bootstrap-rac.sh /bootstrap-rac.sh
RUN chmod +x /bootstrap-rac.sh && /bootstrap-rac.sh && rm /bootstrap-rac.sh 
COPY ansible /ansible/
RUN cd ansible && ansible localhost --connection=local  --module-name include_role --args name=dev && cd .. && rm -r /ansible 

FROM apnpucky/debian-latexmk as latexmk
LABEL maintainer="APN-Pucky"
ADD https://raw.githubusercontent.com/APN-Pucky/rolling-ansible-container/master/bootstrap-rac.sh /bootstrap-rac.sh
RUN chmod +x /bootstrap-rac.sh && /bootstrap-rac.sh && rm /bootstrap-rac.sh 
COPY ansible /ansible/
RUN cd ansible && ansible localhost --connection=local  --module-name include_role --args name=latexmk && cd .. && rm -r /ansible 