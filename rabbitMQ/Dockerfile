FROM rabbitmq:3.10-management-alpine

ENV RABBITMQ_USER user
ENV RABBITMQ_PASSWORD user
ENV RABBITMQ_PID_FILE /var/lib/rabbitmq/mnesia/rabbitmq

ADD init.sh /init.sh
RUN chmod +x /init.sh
EXPOSE 15672

# Define default command
CMD ["/init.sh"]