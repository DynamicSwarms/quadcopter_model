# A quadcopter model in cpp for use with crazyflie quadcopter

This repo includes a cpp quadcopter model which is only dependent on EIGEN library. 
It includes its own solver and uses RK4 for integration.

> **Warning**  
> Read **“Quadrotor Model Based on the Flightmare Simulator”** section.
> The model and code used is derived from Flightmare project.

## Usage for crazyflies 
For crazyflie this configuration can be used: 

```
const Scalar mass = 0.032;
const Scalar arm_length = 0.0325; 
QuadrotorDynamics dynamics(mass, arm_length);
m_quadrotor = std::make_shared<Quadrotor>(dynamics);
QuadState initial_state;
initial_state.setZero();
m_quadrotor->setState(initial_state);
m_quadrotor->setWorldBox((Matrix<3, 2>() << -10, 10, -10, 10, 0.0, 10).finished());
m_cmd.t = 0.0;
m_cmd.thrusts = quadcopter::Vector<4>::Zero();
```

and updated with: 

```
update_thrust_command();
m_cmd.t += 0.001;  // time in seconds
m_quadrotor->run(m_cmd, 0.001);
```

(Thrust needs to be given in Newtons, follow [this guide](https://www.bitcraze.io/documentation/repository/crazyflie-firmware/master/functional-areas/pwm-to-thrust/) to estimate.)

## Other platforms

Follow the Official Flighmare documentation how to set up in general.

# Quadrotor Model Based on the Flightmare Simulator

 This model implementation is derived from:

 - Song, Y., Naji, S., Kaufmann, E., Loquercio, A., & Scaramuzza, D. (2020).  
   *Flightmare: A Flexible Quadrotor Simulator.*  
   Conference on Robot Learning (CoRL) 2020.

 Original project and source code:

 - <https://github.com/uzh-rpg/flightmare>

All credit for the original software design and core modeling ideas belongs to the Flightmare authors.

# Our Modifications:

- Thrust commands and underlying model use Crazyflie Motor numbering
- Quadcopter BodyState updates so it can be used for IMU data
- World Box constraints in z changed slightly
