resource "aws_autoscaling_group" "asg" {
  name                      = "example_asg"
  launch_configuration      = aws_launch_configuration.asg_lc.id
  min_size                  = 1 
  max_size                  = 2 
  desired_capacity          = 2 
  vpc_zone_identifier       = [aws_subnet.private_subnet.id]
  health_check_type         = "EC2"
  health_check_grace_period = 300

  tag {
      key                 = "Name"
      value               = "example-asg"
      propagate_at_period = true
    }
}

resource "aws_launch_configuration" "asg_lc" {
  name             = "example-lc"
  image_id         = "ami-0b419c34b01d1859"
  instance_type    = "t2.micro"
  key_name         = "universal_key"
  security_groups  = [aws_security_group.sg.id]

  user_data = <<-EOF
              #!/bin/bash
              echo "hello, world" > /tmp/hello.txt
              EOF
}
 resource
 